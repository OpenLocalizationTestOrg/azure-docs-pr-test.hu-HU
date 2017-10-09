---
title: "aaaTroubleshooting hello automatikus regisztráció az Azure AD-tartományhoz csatlakoztatott számítógépekre Windows 10 és Windows Server 2016 |} Microsoft Docs"
description: "A Windows 10 és Windows Server 2016 hello automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
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
ms.openlocfilehash: 3795323ce9392368b412b3e1208868431e59a74b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad--windows-10-and-windows-server-2016"></a>Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016

Ez a témakör a következő ügyfelek alkalmazható toohello:

-   Windows 10
-   Windows Server 2016

Egyéb Windows-ügyfelein, lásd: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépek tooAzure AD a Windows régebbi verziójú kliensek](active-directory-device-registration-troubleshoot-windows-legacy.md).

Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-device-registration-get-started.md) a következő forgatókönyvek toosupport hello:

- [Eszközalapú feltételes hozzáférés](active-directory-conditional-access-automatic-device-registration-setup.md)

- [Vállalati központi beállítások](active-directory-windows-enterprise-state-roaming-overview.md)

- [Vállalati Windows Hello](active-directory-azureadjoin-passport-deployment.md)


Ez a dokumentum hogyan tooresolve potenciális problémák nyújt hibaelhárítási útmutatót. 

hello regisztrációs támogatott a hello Windows 2015. November 10. frissítés vagy újabb verzió.  
Hello évforduló frissítés engedélyezéséhez a fenti helyzetekben hello használatát javasoljuk.

## <a name="step-1-retrieve-hello-registration-status"></a>1. lépés: Hello regisztrációs állapotának lekérése 

**tooretrieve hello regisztrációs állapotát:**

1. Nyissa meg a hello parancssort rendszergazdaként.

2. Típus **dsregcmd/status**



    +----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform titkosításszolgáltató TpmProtected: Igen KeySignTest:: futtassa emelt szintű kell tootest.
                  IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-Beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Igen tartománynév: CONTOSO
    
    +----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen



## <a name="step-2-evaluate-hello-registration-status"></a>2. lépés: Hello regisztrációs állapotának kiértékelésére. 

Tekintse át a következő mezők hello, és győződjön meg arról, hogy rendelkezik-e hello várt értékek:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Igen  

Ez a mező jeleníti meg, hogy hello eszköz regisztrálva van-e az Azure ad-val. Hello értéket jeleníti meg, mint a "Nem", ha regisztrációs nem fejeződött be. 

**Lehetséges okok:**

- Hello számítógép regisztrálása nem sikerült a hitelesítés.

- HTTP-proxy van hello olyan szervezet, amely nem észlelhetők hello számítógép

- hello számítógép nem érhető el, az Azure AD hitelesítésében vagy Azure DRS a regisztrációhoz

- hello számítógép nem lehet hello szervezet belső hálózaton vagy a VPN közvetlen tooan a helyszíni AD-tartományvezérlő.

- Ha hello számítógép TPM-mel, valószínűleg rossz állapotban.

- Nem lehet egy helytelen konfiguráció, a szolgáltatások hello dokumentumban korábban feljegyzett, akkor kell tooverify újra. Gyakori példák:

    - Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok

    - Az összevonási kiszolgáló integrált Windows-hitelesítés segítségével a hálózat nem teszi lehetővé bejövő hitelesítés számítógépekről.

    - Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum

---

### <a name="domainjoined--yes"></a>DomainJoined: Igen  

Ez a mező látható, hogy hello eszköz illesztett tooan a helyszíni Active Directory-e. Amennyiben másként hello értéke **nem**, hello eszköz nem automatikus regisztrációját az Azure ad-val. Először ellenőrizze, hogy hello eszköz illesztések toohello a helyszíni Active Directory előtt is regisztrálja az Azure ad-val. Ha hello számítógép tooAzure AD közvetlenül csatlakoztatná keres, nyissa meg az Azure Active Directory csatlakozási képességekre vonatkozó tooLearn.

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: nincs  

Ez a mező jeleníti meg, hogy hello eszköz regisztrálva van-e, és az Azure AD, de (munkahelyhez csatlakoztatott jelölésű) személyes eszközként. Ezt az értéket, a "Nem" jelenjen meg a tartományhoz csatlakoztatott számítógép regisztrálva az Azure AD, azonban azt mutatja, az azt jelenti, hogy Igen, mert egy munkahelyi vagy iskolai fiókot a hozzáadott előzetes toohello számítógép épp regisztrációs volt. Ebben az esetben hello fiókot figyelmen kívül ha Windows 10 (ha hello WinVer parancs futtatása hello a "Futtatás" vagy egy parancssori ablakot 1607) hello évforduló frissítés verzióját használja.

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Igen és AzureADPrt: Igen
  
Ezek a mezők megjelenítése hello felhasználó sikeresen hitelesítette tooAzure AD toohello eszköz aláírásakor. Ha azok megjelenítése "Nem" hello a következők okozhatják:

- Hibás tárolási (STK) TPM hello eszköz regisztrálásakor (ellenőrzés hello KeySignTest emelt szintű futtatása közben) társított kulcs.

- Másodlagos bejelentkezési Azonosítóval

- HTTP-Proxy nem található.

## <a name="next-steps"></a>Következő lépések

További információkért lásd: hello [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md) 
