---
title: "Alkalmazások és az Azure Active Directoryban feltételes hozzáférési szabályai használó |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi, a megadott feltételeket, amikor a felhasználó, és lehetővé teszi az alkalmazások elérésére."
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
ms.openlocfilehash: 8614660f7c98af7b4e6d50348775495c67ae1cc8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="applications-and-browsers-that-use-conditional-access-rules-in-azure-active-directory"></a>Alkalmazások és a feltételes hozzáférési szabályai használó Azure Active Directoryban

Feltételes hozzáférési szabályai támogatottak az Azure Active Directory (Azure AD)-alkalmazásokhoz kapcsolódó, előre integrált összevont szoftver egy szolgáltatott szoftverként (SaaS) alkalmazások, a jelszót az egyszeri bejelentkezés (SSO), az üzletági alkalmazások és az alkalmazásokat, amelyek használják az Azure AD-alkalmazásproxy használó alkalmazásokat. Alkalmazások, amelyek használhatja a feltételes hozzáférés részletes listájáért lásd: [feltételes hozzáférésű engedélyezett szolgáltatások](active-directory-conditional-access-technical-reference.md). Feltételes hozzáférés a modern hitelesítést használó mobil- és asztali alkalmazások is működik. Ebben a cikkben azt fedik le a feltételes hozzáférés működését, mobil- és asztali alkalmazásokban.

A modern hitelesítést használó alkalmazások az Azure AD bejelentkezési lapok is használhatja. A bejelentkezési oldal, a rendszer a multi-factor authentication. Üzenet is megjelenik, ha a felhasználó hozzáférése blokkolva van. Modern hitelesítést hitelesítéséhez az Azure ad-vel, az eszköz szükség, így az eszközalapú feltételes hozzáférési házirendek kiértékelése.

Fontos tudni, hogy mely alkalmazások használhatja a feltételes hozzáférési szabályai, és a lépéseket, amelyek hasznosak lehetnek a más alkalmazás belépési pontok biztonságossá tenni.

## <a name="applications-that-use-modern-authentication"></a>A modern hitelesítést használó alkalmazások
> [!NOTE]
> Ha egy feltételes hozzáférési szabályzatot az Azure ad-ben, amely rendelkezik egy azzal egyenértékű az Office 365-ben, mindkét feltételes hozzáférési házirend konfigurálása Ez akkor vonatkozik, például az Exchange Online vagy SharePoint Online feltételes hozzáférési szabályzatok.
>
>

Az alábbi alkalmazások támogatják a feltételes hozzáférés az Office 365 és más Azure AD-kapcsolódó szolgáltatás alkalmazáshoz:


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
| Az Office 365 SharePoint online szolgáltatáshoz| Windows 10| Office 2016-alkalmazások, univerzális Office-alkalmazásokat, Office 2013 (a modern hitelesítést), onedrive vállalati verzió szinkronizálási ügyfél (lásd: [megjegyzések](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), a jövőben, SharePoint-alkalmazások támogatása a jövőben tervezett tervezett Office csoportok támogatása|
| Az Office 365 SharePoint online szolgáltatáshoz| Windows 8.1, Windows 7| Office 2016, Office 2013 (a modern hitelesítést), a onedrive vállalati verzió alkalmazások szinkronizálása ügyfél (lásd: [megjegyzések](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|
| Az Office 365 SharePoint online szolgáltatáshoz| iOS, Android| Office-mobilalkalmazások|
| Az Office 365 SharePoint online szolgáltatáshoz| Mac OS X| Office 2016 macOS (Word, Excel, PowerPoint, csak a OneNote). A OneDrive vállalati verziójának ügyfélszolgálatával tervezett a jövőben|
| Az Office 365 Yammer| Windows 10, iOS, Android| Office Yammer-alkalmazás|
| Power bi szolgáltatás| Windows 10, Windows 8.1, Windows 7 és iOS| Power bi alkalmazásról. Az Androidhoz készült Power BI alkalmazás jelenleg nem támogatja a eszközalapú feltételes hozzáférés.|
| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS és Android| A Visual Studio Team Services-alkalmazás|








## <a name="applications-that-do-not-use-modern-authentication"></a>A nem modern hitelesítést használó alkalmazások
Jelenleg más módszerrel kell használnia blokkolni a hozzáférést a nem modern hitelesítést használó alkalmazásokhoz. Hozzáférési szabályok nem modern hitelesítést használó alkalmazások nem kényszeríti ki a feltételes hozzáférés. Ez elsősorban az Exchange és SharePoint hozzáférést veszi figyelembe. A legtöbb korábbi verziója régebbi hozzáférést vezérlő protokollt használják.

### <a name="control-access-in-office-365-sharepoint-online"></a>Az Office 365 SharePoint Online hozzáférés szabályozása
A Set-SPOTenant parancsmaggal letilthatja örökölt protokollok a SharePoint-hozzáféréshez. Ez a parancsmag segítségével megelőzése az Office-ügyfelek, amelyek nem modern hitelesítési protokollok nem férhet hozzá a SharePoint Online-erőforrások.

**Példaparancs**:`Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Elérés az Office 365 Exchange Online
Exchange protokollok két fő kategóriába kínál. Tekintse át a következő beállításokat, majd válassza ki a házirendet, amely a szervezet számára megfelelő.

* **Az Exchange ActiveSync**. Alapértelmezés szerint feltételes hozzáférési szabályzatokat a többtényezős hitelesítés és a hely nem kényszeríti ki az Exchange ActiveSync szolgáltatást. Szeretne védeni a szolgáltatásokhoz való hozzáférés, közvetlenül az Exchange ActiveSync-házirend konfigurálása, vagy az Exchange ActiveSync blokkolja az Active Directory összevonási szolgáltatások (AD FS) vonatkozó szabályok használatával.
* **Az örökölt protokollok**. Az AD FS örökölt protokollok tilthatja le. A régebbi Office-ügyfelek, például az Office 2013 modern hitelesítés engedélyezve van, és az Office korábbi verziói nem blokkolja a hozzáférést.

### <a name="use-ad-fs-to-block-legacy-protocol"></a>Az AD FS segítségével örökölt protokoll letiltása
A következő példa kiállítási engedélyezésszabályok használhatja az Active Directory összevonási szolgáltatások szintjén örökölt protokoll hozzáférésének letiltására. Két általános konfigurációk közül választhat.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>1. lehetőség: Az Exchange ActiveSync engedélyezése, és engedélyezi, hogy az örökölt alkalmazások, de csak az intraneten
A következő három szabályok alkalmazásával az AD FS megbízható függő entitás a Microsoft Office 365 Identitásplatformmal, az Exchange ActiveSync-forgalmat, és a böngésző és a modern hitelesítési forgalom, hozzáféréssel rendelkeznek. Az örökölt alkalmazások sem az extranetről.

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
A következő három szabályok alkalmazásával az AD FS megbízható függő entitás a Microsoft Office 365 Identitásplatformmal, az Exchange ActiveSync-forgalmat, és a böngésző és a modern hitelesítési forgalom, hozzáféréssel rendelkeznek. Az örökölt alkalmazások egyik helyen sem.

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

Csak kaphat hozzáférést, hogy ellenőrzés-alapú eszköz-házirendek az eszköz megfelelőségét és a tartományhoz való csatlakozást amikor az Azure AD azonosítja, és az eszköz hitelesítésére. A legtöbb ellenőrzések, például a helyét és a legtöbb eszköz és a böngészők, MFA munka során eszközökre vonatkozó házirendeket az operációs rendszer verziója és az alábbi böngészők szükséges. Hozzáférés a felhasználók számára nem támogatott böngészők vagy az operációs rendszerek blokkolva van, ha egy eszköz házirend van beállítva. 

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
> Chrome támogatásához, Windows 10 Creators frissítés használata és telepítése a kiterjesztés található [Itt](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).
>
>

## <a name="next-steps"></a>Következő lépések

További részletekért lásd: [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-supported-apps/ic195031.png


