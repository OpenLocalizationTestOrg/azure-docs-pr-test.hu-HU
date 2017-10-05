---
title: "Hibaelhárítás az automatikus regisztráció az Azure AD-tartomány csatlakoztatott számítógépek Windows 10 és Windows Server 2016 |} Microsoft Docs"
description: "A Windows 10 és Windows Server 2016 automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 5b7f95f302f716d9221b5fae59aa2df5c956a524
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a>Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépeit az Azure AD – a Windows 10 és Windows Server 2016

Ez a témakör a következő alkalmazható a következő ügyfelekre:

-   Windows 10
-   Windows Server 2016

Egyéb Windows-ügyfelein, lásd: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépeit az Azure AD Windows régebbi ügyfelek](active-directory-device-registration-troubleshoot-windows-legacy.md).

Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-device-registration-get-started.md) támogatásához a következő esetekben:

- [Eszközalapú feltételes hozzáférés](active-directory-conditional-access-automatic-device-registration-setup.md)

- [Vállalati központi beállítások](active-directory-windows-enterprise-state-roaming-overview.md)

- [Vállalati Windows Hello](active-directory-azureadjoin-passport-deployment.md)


Ez a dokumentum a lehetséges problémák megoldásához nyújt hibaelhárítási útmutatót. 

A regisztrációt a Windows támogatott 2015. November 10. frissítés vagy újabb verzió.  
A évforduló frissítés engedélyezéséhez a fenti helyzetekben használatát javasoljuk.

## <a name="step-1-retrieve-the-registration-status"></a>1. lépés: A regisztráció állapotának lekérése 

**A regisztrációs állapot beolvasása:**

1. Nyissa meg a parancssort rendszergazdaként.

2. Típus **dsregcmd/status**



    +----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform titkosításszolgáltató TpmProtected: Igen KeySignTest:: emelt szintű tesztelése kell futtatnia.
                  IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-Beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Igen tartománynév: CONTOSO
    
    +----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen



## <a name="step-2-evaluate-the-registration-status"></a>2. lépés: A regisztráció állapotának kiértékelésére. 

Tekintse át a következő mezőket, és győződjön meg arról, hogy rendelkezik-e a várt értékek:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Igen  

Ez a mező jeleníti meg, hogy az eszköz regisztrálva van-e az Azure ad-val. Amennyiben az értéke, a "Nem", regisztrációs nem fejeződött be. 

**Lehetséges okok:**

- A számítógép regisztrációjához hitelesítés sikertelen.

- Nincs olyan szervezet, amely a számítógép nem észlelhetők egy HTTP-proxy

- A számítógép nem érhető el, az Azure AD hitelesítésében vagy Azure DRS a regisztrációhoz

- A számítógép nem lehet a szervezet belső hálózaton vagy a VPN a közvetlen sor a láthatáron egy a helyszíni AD-tartományvezérlő.

- Ha a számítógép TPM-mel, valószínűleg rossz állapotban.

- Előfordulhat, a szolgáltatások egy helytelen konfiguráció című dokumentumban korábban feljegyzett, hogy ellenőrizze újra kell. Gyakori példák:

    - Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok

    - Az összevonási kiszolgáló integrált Windows-hitelesítés segítségével a hálózat nem teszi lehetővé bejövő hitelesítés számítógépekről.

    - Nincs szolgáltatáskapcsolódási pont objektum mutat, a ellenőrzött tartomány nevét az Azure ad-ben az Active Directory-erdőben, ahol a számítógép tartozik

---

### <a name="domainjoined--yes"></a>DomainJoined: Igen  

Ez a mező jeleníti meg, hogy az eszköz csatlakozott a helyi Active Directory-e. Amennyiben az értéke **nem**, az eszköz nem automatikus regisztrációját az Azure ad-val. Először ellenőrizze a, hogy az eszköz csatlakozik a helyszíni Active Directory előtt is regisztrálja az Azure ad-val. Ha a számítógép csatlakoztatása az Azure AD közvetlenül keres, lépjen további információ az Azure Active Directory csatlakozási képességeivel kapcsolatos.

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: nincs  

Ez a mező jeleníti meg, hogy az Azure ad-vel, de (munkahelyhez csatlakoztatott jelölésű) személyes eszközként regisztrálja az eszközt. Ez az érték "Nem", a tartományhoz csatlakozó számítógépen az Azure ad-vel regisztrált jelenjen meg, azonban ha igen, mert mutat azt jelenti, hogy a munkahelyi vagy iskolai fiókkal lett hozzáadva a számítógép regisztráció befejezése előtt. Ebben az esetben a fiók lesz figyelembe véve, ha a Windows 10 (Ha a "Futtatás" vagy a parancssori ablakban futó a WinVer parancs 1607) évforduló frissítés verzióját használja.

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Igen és AzureADPrt: Igen
  
Ezek a mezők megjelenítése, hogy az Azure AD-követően az eszközre való bejelentkezéskor rendelkezik sikeresen hitelesíteni a felhasználót. Ha ezek megjelenítése "Nem", a következők okozhatják:

- Hibás tárolási kulcs (STK) az eszköz regisztrálásakor (Ellenőrizze az emelt szintű futtatása közben KeySignTest) társított TPM.

- Másodlagos bejelentkezési Azonosítóval

- HTTP-Proxy nem található.

## <a name="next-steps"></a>Következő lépések

További információkért lásd: a [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md) 