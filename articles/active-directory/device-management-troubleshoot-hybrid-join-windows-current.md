---
title: "aaaTroubleshooting hibrid Azure Active Directoryhoz csatlakoztatott Windows 10 és Windows Server 2016-os eszközök |} Microsoft Docs"
description: "Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott Windows 10 és Windows Server 2016-os eszközök."
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
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc252d1d0684d6632694afc8a367327794228c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott Windows 10 és Windows Server 2016-os eszközök 

Ez a témakör a következő ügyfelek alkalmazható toohello:

-   Windows 10
-   Windows Server 2016

Egyéb Windows-ügyfelein, lásd: [hibaelhárítás hibrid Azure Active Directoryhoz csatlakoztatott régebbi eszközök](device-management-troubleshoot-hybrid-join-windows-legacy.md).

Ez a témakör azt feltételezi, hogy [konfigurált hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello a következő esetekben:

- Eszközalapú feltételes hozzáférés

- [Vállalati központi beállítások](active-directory-windows-enterprise-state-roaming-overview.md)

- [Vállalati Windows Hello](active-directory-azureadjoin-passport-deployment.md)


Ez a dokumentum hogyan tooresolve potenciális problémák nyújt hibaelhárítási útmutatót. 


Windows 10 és Windows Server 2016, az Azure Active Directory join támogatja hello Windows hibrid 2015. November 10. Frissítse vagy újabb verzió. Hello évforduló frissítés használatát javasoljuk.

## <a name="step-1-retrieve-hello-join-status"></a>1. lépés: Hello illesztési állapotának lekérése 

**tooretrieve hello illesztési állapota:**

1. Nyissa meg hello parancssort rendszergazdaként

2. Típus **dsregcmd/status**



    +----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+
    
        AzureAdJoined: YES
     EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform titkosításszolgáltató TpmProtected: Igen KeySignTest:: futtassa emelt szintű kell tootest.
                  IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Igen tartománynév: CONTOSO
    
    +----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen



## <a name="step-2-evaluate-hello-join-status"></a>2. lépés: Hello illesztési állapotának kiértékelésére. 

Tekintse át a következő mezők hello, és győződjön meg arról, hogy rendelkezik-e hello várt értékek:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Igen  

Ez a mező jelzi, hogy hello eszköz csatlakozott-e az Azure ad-val. Ha hello érték **nem**, hello illesztési tooAzure AD még nem fejeződött be. 

**Lehetséges okok:**

- Az illesztés hello számítógép hitelesítés sikertelen.

- HTTP-proxy van hello olyan szervezet, amely nem észlelhetők hello számítógép

- hello számítógép nem érhető el az Azure AD tooauthenticate vagy Azure DRS a regisztrációhoz

- hello számítógép nem lehet hello szervezet belső hálózaton vagy a VPN közvetlen tooan a helyszíni AD-tartományvezérlő.

- Ha hello számítógép TPM-mel, az állapota lehet.

- Valószínűleg a helytelen konfiguráció hello szolgáltatásban hello dokumentumban korábban feljegyzett, akkor kell tooverify újra. Gyakori példák:

    - Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok

    - Az összevonási kiszolgálón nem engedélyezi a bejövő hitelesítést számítógépekről integrált Windows-hitelesítés segítségével a hálózaton.

    - Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum

---

### <a name="domainjoined--yes"></a>DomainJoined: Igen  

Ez a mező azt jelzi, hogy hello eszköz tooan a helyszíni Active Directory-e. Ha hello érték **nem**, hello eszköz nem tudja végrehajtani a Azure AD hibrid csatlakozzon.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: nincs  

Ez a mező jelzi, hogy hello eszköz regisztrálva van-e az Azure AD egy személyes eszközként (jelölésű *munkahelyhez csatlakoztatott*). Ez az érték legyen **nem** egy tartományhoz csatlakozó számítógép, amely egyúttal az Azure AD hibrid csatlakoztatva. Ha hello érték **Igen**, a munkahelyi vagy iskolai fiók hozzá lett adva hello hibrid az Azure AD join előzetes toohello befejezését. Ebben az esetben hello fiók rendszer figyelmen kívül hagyja a Windows 10 (1607) hello évforduló frissítés verzióját használja.

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Igen és AzureADPrt: Igen
  
Ezek a mezők azt jelzi, hogy hello felhasználó sikeresen hitelesített tooAzure AD toohello eszköz bejelentkezéskor. Ha hello értékek **nem**, annak oka az lehet esedékes:

- Hibás tárolási (STK) TPM hello eszköz regisztrálásakor (ellenőrzés hello KeySignTest emelt szintű futtatása közben) társított kulcs.

- Másodlagos bejelentkezési Azonosítóval

- HTTP-Proxy nem található.

## <a name="next-steps"></a>Következő lépések

A kérdésekhez lásd: hello [eszköz felügyeleti kapcsolatos gyakori kérdések](device-management-faq.md) 
