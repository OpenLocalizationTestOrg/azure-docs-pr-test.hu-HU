---
title: "aaaApplications és az Azure Active Directoryban feltételes hozzáférési szabályai használó |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi a megadott feltételek amikor hello felhasználói és tooallow alkalmazás-hozzáférés."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ca20853bb9f4b22d0b88ddd2f051d658e0d88cf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="applications-and-browsers-that-use-conditional-access-rules-in-azure-active-directory"></a>Alkalmazások és a feltételes hozzáférési szabályai használó Azure Active Directoryban

Feltételes hozzáférési szabályai támogatottak az Azure Active Directory (Azure AD)-alkalmazásokhoz kapcsolódó, előre integrált összevont szoftver egy szolgáltatott szoftverként (SaaS) alkalmazások, a jelszót az egyszeri bejelentkezés (SSO), az üzletági alkalmazások és az alkalmazásokat, amelyek használják az Azure AD-alkalmazásproxy használó alkalmazásokat. Alkalmazások, amelyek használhatja a feltételes hozzáférés részletes listájáért lásd: [feltételes hozzáférésű engedélyezett szolgáltatások](active-directory-conditional-access-technical-reference.md). Feltételes hozzáférés a modern hitelesítést használó mobil- és asztali alkalmazások is működik. Ebben a cikkben azt fedik le a feltételes hozzáférés működését, mobil- és asztali alkalmazásokban.

A modern hitelesítést használó alkalmazások az Azure AD bejelentkezési lapok is használhatja. A bejelentkezési oldal, a rendszer a multi-factor authentication. Egy üzenet is megjelenik, ha hello felhasználónak nincs hozzáférése. Modern hitelesítést hello eszköz tooauthenticate az Azure ad-vel, szükség, így az eszközalapú feltételes hozzáférési házirendek kiértékelése.

Fontos tooknow, mely alkalmazások használhatja a feltételes hozzáférési szabályai, és a lépéseket, hogy szükség lehet tootake toosecure más alkalmazás belépési pontok hello.

## <a name="applications-that-use-modern-authentication"></a>A modern hitelesítést használó alkalmazások
> [!NOTE]
> Ha egy feltételes hozzáférési szabályzatot az Azure ad-ben, amely rendelkezik egy azzal egyenértékű az Office 365-ben, mindkét feltételes hozzáférési házirend konfigurálása Ez akkor érvényes, például tooconditional hozzáférési házirendek az Exchange Online vagy SharePoint online-hoz.
>
>

hello következő alkalmazások támogatják a feltételes hozzáférés az Office 365 és más Azure AD-csatlakoztatott szolgáltatásalkalmazások:


| Célként megadott szolgáltatás| Platform| Alkalmazás |
| --- | --- | --- |
| Bármely saját alkalmazások app service| Android és iOS| Többtényezős hitelesítés és a hely házirend-alkalmazásokhoz. Eszköz alapú szabályzatok nem támogatottak.|
| Az Azure távoli App service| Windows 10, Windows 8.1, Windows 7, iOS, Android és Mac OS X| Azure távoli alkalmazás|
| Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS és Android| Dynamics CRM-alkalmazás|
| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS vagy Android- és MAC OS x| Microsoft csapatok szolgáltatások – ezen lehetőség, amely támogatja a Microsoft Teams minden szolgáltatás és az ügyfél alkalmazások - Windows asztali, MAC OS X, iOS, Android, WP és webes ügyfél|
| Az Office 365 Exchange online-hoz| Windows 10| Mail/naptár/személyek alkalmazást, és az Outlook 2016, az Outlook 2013 (modern hitelesítést), a Skype vállalati (a modern hitelesítést)|
| Az Office 365 Exchange online-hoz| Windows 8.1, Windows 7| Outlook 2016, az Outlook 2013 (modern hitelesítést), a Skype vállalati verzió (a modern hitelesítést)|
| Az Office 365 Exchange online-hoz| iOS| Outlook mobilalkalmazás|
| Az Office 365 Exchange online-hoz| Mac OS X| Outlook 2016 (Office macOS)|
| Az Office 365 SharePoint online szolgáltatáshoz| Windows 10| Office 2016-alkalmazások, univerzális Office-alkalmazásokat, Office 2013 (modern hitelesítést), a onedrive vállalati verzió szinkronizálási ügyfél (lásd: [megjegyzések](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), a tervezett jövőbeli hello Office csoportok támogatása, SharePoint-alkalmazások támogatása a tervezett jövőbeli hello|
| Az Office 365 SharePoint online szolgáltatáshoz| Windows 8.1, Windows 7| Office 2016, Office 2013 (a modern hitelesítést), a onedrive vállalati verzió alkalmazások szinkronizálása ügyfél (lásd: [megjegyzések](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|
| Az Office 365 SharePoint online szolgáltatáshoz| iOS, Android| Office-mobilalkalmazások|
| Az Office 365 SharePoint online szolgáltatáshoz| Mac OS X| Office 2016 macOS (Word, Excel, PowerPoint, csak a OneNote). A OneDrive vállalati verziójának ügyfélszolgálatával a tervezett jövőbeli hello|
| Az Office 365 Yammer| Windows 10, iOS, Android| Office Yammer-alkalmazás|
| Power bi szolgáltatás| Windows 10, Windows 8.1, Windows 7 és iOS| Power bi alkalmazásról. Androidhoz készült Power BI alkalmazás hello jelenleg nem támogatja a eszközalapú feltételes hozzáférés.|
| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS és Android| A Visual Studio Team Services-alkalmazás|








## <a name="applications-that-do-not-use-modern-authentication"></a>A nem modern hitelesítést használó alkalmazások
Jelenleg más módszerek tooblock hozzáférés tooapps, amelyek nem használják a modern hitelesítést kell használnia. Hozzáférési szabályok nem modern hitelesítést használó alkalmazások nem kényszeríti ki a feltételes hozzáférés. Ez elsősorban az Exchange és SharePoint hozzáférést veszi figyelembe. A legtöbb korábbi verziója régebbi hozzáférést vezérlő protokollt használják.

### <a name="control-access-in-office-365-sharepoint-online"></a>Az Office 365 SharePoint Online hozzáférés szabályozása
A SharePoint-hozzáféréshez örökölt protokollok hello Set-SPOTenant parancsmaggal letilthatja. A parancsmag tooprevent Office-ügyfelekhez, amelyek nem modern hitelesítési protokollok nem férhet hozzá a SharePoint Online-erőforrások használatára.

**Példaparancs**:`Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Elérés az Office 365 Exchange Online
Exchange protokollok két fő kategóriába kínál. Tekintse át az alábbi beállítások hello, és válassza ki a szervezet számára megfelelő hello házirend.

* **Az Exchange ActiveSync**. Alapértelmezés szerint feltételes hozzáférési szabályzatokat a többtényezős hitelesítés és a hely nem kényszeríti ki az Exchange ActiveSync szolgáltatást. Meg kell tooprotect toothese szolgáltatás közvetlenül az Exchange ActiveSync-házirend konfigurálása, vagy az Exchange ActiveSync blokkolja az Active Directory összevonási szolgáltatások (AD FS) vonatkozó szabályok használatával.
* **Az örökölt protokollok**. Az AD FS örökölt protokollok tilthatja le. Ez megakadályozza, hogy a hozzáférés tooolder Office-ügyfelek, például az Office 2013 modern hitelesítés engedélyezve van, és az Office korábbi verziói nélkül.

### <a name="use-ad-fs-tooblock-legacy-protocol"></a>Az AD FS tooblock örökölt protokoll
A következő példa kiállítási engedélyezési szabályok tooblock örökölt protokoll hozzáférést AD FS hello szinten hello is használhatja. Két általános konfigurációk közül választhat.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-hello-intranet"></a>1. lehetőség: Az Exchange ActiveSync engedélyezése, és annak engedélyezése, hogy az örökölt alkalmazások, de csak hello intranet
Úgy, hogy alkalmazza a következő három szabályok toohello hello AD FS megbízható függő megbízik a Microsoft Office 365-Identitásplatformmal, az Exchange ActiveSync-forgalom esetén, és a böngésző és a modern hitelesítési forgalom fér hozzá. Az örökölt alkalmazások extranetes hello sem.

##### <a name="rule-1"></a>1. szabály
    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>A 2. szabály
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>3. szabály
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>2. lehetőség: Lehetővé teszi az Exchange ActiveSync, és az örökölt alkalmazások blokkolása
Úgy, hogy alkalmazza a következő három szabályok toohello hello AD FS megbízható függő megbízik a Microsoft Office 365-Identitásplatformmal, az Exchange ActiveSync-forgalom esetén, és a böngésző és a modern hitelesítési forgalom fér hozzá. Az örökölt alkalmazások egyik helyen sem.

##### <a name="rule-1"></a>1. szabály
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>A 2. szabály
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>3. szabály
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


## <a name="supported-browsers-for-device-based-policies"></a>Támogatott böngészők eszköz alapú házirendek 

Csak kaphat hozzáférés-alapú eszköz-házirendek, ellenőrizze az eszköz megfelelőségét és a tartományhoz való csatlakozást az Azure AD is azonosíthatja és hello eszköz hitelesítéséhez. A legtöbb ellenőrzések, például a helyét és a legtöbb eszköz és a böngészők, MFA munka során eszközökre vonatkozó házirendeket hello az operációs rendszer verziója és az alábbi böngészők szükséges. Hozzáférés a felhasználók számára nem támogatott böngészők vagy hello operációs rendszereken blokkolva van, ha egy eszköz házirend van beállítva. 

| Operációs rendszer                     | Böngészők                 | Támogatás     |
| :--                    | :--                      | :-:         |
| Win 10                 | IE a peremhálózati                 | ![Jelölőnégyzet][1] |
| Win 10                 | Chrome                   | Előzetes verzió     |
| Win 8 / 8.1            | IE Chrome               | ![Jelölőnégyzet][1] |
| Win 7                  | IE Chrome               | ![Jelölőnégyzet][1] |
| iOS                    | Safari                   | ![Jelölőnégyzet][1] |
| Android                | Chrome                   | ![Jelölőnégyzet][1] |
| Windows Phone          | IE a peremhálózati                 | ![Jelölőnégyzet][1] |
| Windows Server 2016    | IE a peremhálózati                 | ![Jelölőnégyzet][1] |
| Windows Server 2016    | Chrome                   | Hamarosan |
| Windows Server 2012 R2 | IE Chrome               | ![Jelölőnégyzet][1] |
| Windows Server 2008 R2 | IE Chrome               | ![Jelölőnégyzet][1] |
| Mac OS                 | Safari                   | ![Jelölőnégyzet][1] |
| Mac OS                 | Chrome                   | Hamarosan |

> [!NOTE]
> A Chrome támogatáshoz kell használnia a Windows 10 Creators frissítés és telepítés hello kiterjesztés található [Itt](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).
>
>

## <a name="next-steps"></a>Következő lépések

További részletekért lásd: [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-supported-apps/ic195031.png


