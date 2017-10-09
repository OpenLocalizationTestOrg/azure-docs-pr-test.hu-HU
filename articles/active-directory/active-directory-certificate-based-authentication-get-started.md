---
title: "aaaAzure Active Directory-alapú hitelesítés – első lépések |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure tanúsítvány alapú hitelesítést a környezetben"
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 3c73bdf56018c0716085c923a61e9560dbe4004c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Ismerkedés az Azure Active Directory-alapú hitelesítés

Tanúsítvány alapú hitelesítés lehetővé teszi az Exchange online-fiókját az kapcsolódáskor egy Windows-, Android vagy iOS eszközön ügyféltanúsítvánnyal az Azure Active Directory hitelesített toobe: 

- Office-mobilalkalmazások például a Microsoft Outlook és a Microsoft Word   

- Exchange ActiveSync (EAS) ügyfeleket 

Ez a funkció beállítása nem hello kell tooenter egy felhasználónév és jelszó kombináció bizonyos mail és a Microsoft Office-alkalmazásokat a mobileszközön. 

Ez a témakör:

- Hello való tooconfigure lépések és egyszeri Tanúsítványalapú hitelesítés a felhasználók számára a bérlő az Office 365 Enterprise, vállalati, oktatási és az Egyesült Államok kormánya tervek biztosít. Ez a funkció érhető el az előzetes verzió az Office 365 Kína, a US Government védelmet és a US Government Szövetségi csomagokban. 

- Feltételezi, hogy már rendelkezik egy [nyilvános kulcsokra épülő infrastruktúra (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) és [AD FS](connect/active-directory-aadconnectfed-whatis.md) konfigurálva.    


## <a name="requirements"></a>Követelmények

tooconfigure tanúsítvány alapú hitelesítést hello alábbiaknak igaznak kell lenniük:  

- Tanúsítvány alapú hitelesítést (CBA) csak külső környezeteket böngészőalkalmazásokban vagy natív modern hitelesítést (ADAL) használó ügyfelek esetén támogatott. hello egyetlen kivétel ez alól az Exchange Active Sync (EAS) a EXO, mind a összevont, és a felügyelt fiók használható. 

- hello legfelső szintű hitelesítésszolgáltatót és köztes hitelesítésszolgáltatót kell állítani az Azure Active Directoryban.  

- Minden egyes hitelesítésszolgáltató visszavont tanúsítványok listájának (CRL), amely az Internet felé néző URL-cím használatával lehet hivatkozni kell rendelkeznie.  

- Rendelkeznie kell legalább egy hitelesítésszolgáltatót az Azure Active Directoryban konfigurált. Hello található kapcsolódó lépések [hello tanúsítványszolgáltatók konfigurálása](#step-2-configure-the-certificate-authorities) szakasz.  

- Exchange ActiveSync-ügyfelek hello ügyféltanúsítvány hello felhasználói irányítható e-mail cím az Exchange online vagy hello egyszerű felhasználónév vagy hello hello tulajdonos alternatív neve mező értékének RFC822 nevét kell rendelkeznie. Az Azure Active Directory leképezi a hello RFC822 érték toohello proxycím attribútum hello könyvtárban.  

- Az ügyfél hozzáférési tooat legalább egy hitelesítésszolgáltató által kiállított tanúsítványt kell rendelkezniük.  

- Ügyféltanúsítványt az ügyfél-hitelesítést kell kiállították tooyour ügyfél.  




## <a name="step-1-select-your-device-platform"></a>1. lépés: Válassza ki az eszközplatformhoz

Első lépésként hello eszközplatform, az Ön számára legfontosabb kell tooreview hello következő:

- hello Office-mobilalkalmazás támogatja 
- hello adott megvalósítás követelményei  

hello kapcsolatos információkat a következő eszközplatformokat hello létezik:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-hello-certificate-authorities"></a>2. lépés: Hello tanúsítványszolgáltatók konfigurálása 

tooconfigure a hitelesítésszolgáltatók minden tanúsítvány-szolgáltató az Azure Active Directoryban, töltse fel a következő hello: 

* a hello tanúsítvány nyilvános részét hello *.cer* formátumban 
* hello internetre irányuló URL-címeket, ahol hello visszavont tanúsítványok listájának (CRL) találhatók

hello sémát a hitelesítésszolgáltatótól a következőképpen néz ki: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 

Hello konfigurációs hello használhatja [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0):  

1. Indítsa el a Windows PowerShell rendszergazdai jogosultságokkal. 
2. Hello Azure AD-modul telepítése. Tooinstall verziót kell [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) vagy újabb verzióját.  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

Első lépésként konfigurációs tooestablish kapcsolatot és a bérlő kell. Amint létezik egy kapcsolat tooyour bérlői, áttekintheti, adja hozzá, törlése és módosítsa a könyvtárat a definiált hello megbízható hitelesítésszolgáltatók. 

### <a name="connect"></a>Kapcsolódás

a bérlő használata hello kapcsolatot tooestablish [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) parancsmagot:

    Connect-AzureAD 


### <a name="retrieve"></a>Beolvasása 

tooretrieve hello megbízható hitelesítésszolgáltatók a könyvtárban, meghatározott hello használata [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmag. 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a>Hozzáadás

megbízható hitelesítésszolgáltató toocreate hello használata [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmag és a set hello **Vlelérésihelye** attribútum tooa megfelelő érték: 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF hello CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a>Eltávolítás

megbízható hitelesítésszolgáltató tooremove hello használata [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmagot:
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a>Modfiy

megbízható hitelesítésszolgáltató toomodify hello használata [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmagot:

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a>3. lépés: A visszavont tanúsítványok konfigurálása

egy ügyféltanúsítványt, az Azure Active Directory toorevoke beolvassa hello tanúsítványt a visszavont tanúsítványok listáját (CRL) a hello URL hitelesítésszolgáltató adatai részeként feltöltött és gyorsítótárba helyezi azt. hello utolsó közzétételi időbélyeg (**dátumig** tulajdonság) a tanúsítvány-visszavonási listát használt hello tooensure hello CRL Elérési útja még érvényes. tanúsítvány-visszavonási listát hello rendszeresen hivatkozott toorevoke hozzáférés toocertificates hello lista egy részét képező.

Ha több azonnali visszavonás szükséges (például, ha a felhasználó elveszíti a eszköz), érvénytelenítve lenne hello engedélyezési jogkivonat hello felhasználó. hello engedélyezési jogkivonat, beállíthatja hello tooinvalidate **StsRefreshTokenValidFrom** mezőt az adott felhasználó Windows PowerShell használatával. Frissítenie kell a hello **StsRefreshTokenValidFrom** mezőt, minden felhasználóhoz toorevoke hozzáférést szeretne.

hello visszavonási továbbra is fennáll, tooensure, be kell hello **dátumig** hello CRL tooa dátum után értéke hello **StsRefreshTokenValidFrom** , és ellenőrizze, hogy a szóban forgó hello tanúsítvány hello CRL-t.

a következő lépéseket vázlat hello folyamat frissítésére és hello engedélyezési jogkivonat érvénytelenítése által hello beállítása hello **StsRefreshTokenValidFrom** mező. 

**tooconfigure visszavonási:** 

1. Csatlakozás a rendszergazdai hitelesítő adatok toohello MSOL szolgáltatással: 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. Lekéri az hello aktuális StsRefreshTokensValidFrom értéket a felhasználóhoz: 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. Adja meg egy új StsRefreshTokensValidFrom értéket hello felhasználói egyenlő toohello aktuális időbélyeg: 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

beállíthatja hello dátuma jövőbeli hello kell lennie. Ha hello dátuma jövőbeli hello van, hello **StsRefreshTokensValidFrom** tulajdonsága nincs beállítva. Ha hello dátuma jövőbeli, hello **StsRefreshTokensValidFrom** értéke toohello aktuális idő (nem hello dátum szerint Set-MsolUser paranccsal). 


## <a name="step-4-test-your-configuration"></a>4. lépés: A konfiguráció tesztelése

### <a name="testing-your-certificate"></a>A tanúsítvány tesztelése

Első konfiguráció teszteléséhez, mint-t érdemes kipróbálni a toosign túl[Outlook Web Access](https://outlook.office365.com) vagy [SharePoint Online](https://microsoft.sharepoint.com) használatával a **eszközökön böngészőben**.

Ha a bejelentkezés sikeres, akkor tudja, hogy:

- hello felhasználói tanúsítvány már kiépített tooyour vizsgálati eszköz
- Az AD FS megfelelően van konfigurálva.  


### <a name="testing-office-mobile-applications"></a>Office-mobilalkalmazások tesztelése

**tootest Tanúsítványalapú hitelesítés az Office-mobilalkalmazás:** 

1. A vizsgálati eszköz telepítse az Office-mobilalkalmazás (például a OneDrive).
3. Indítsa el a hello alkalmazás. 
4. Adja meg a felhasználónevét, majd válassza ki a kívánt toouse hello felhasználói tanúsítványt. 

Akkor érdemes lehet sikeres volt. 

### <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync ügyfél alkalmazások tesztelése

Exchange ActiveSync (EAS) tooaccess tanúsítvány alapú hitelesítést, az EAS-profil hello ügyfél tanúsítványát tartalmazó keresztül elérhető toohello alkalmazás kell lennie. 

EAS-profil hello hello a következő információkat kell tartalmaznia:

- hello felhasználói tanúsítvány toobe hitelesítéshez használt 

- hello EAS végpont (például outlook.office365.com)

Az EAS-profil konfigurálhatók és elhelyezett hello eszközön keresztül hello mobileszköz-kezelés (MDM) például az Intune-ban, vagy úgy manuálisan hello tanúsítvány hello EAS-profil hello eszközön-felhasználását.  

### <a name="testing-eas-client-applications-on-android"></a>Az Android EAS-ügyfél alkalmazások tesztelése

**tanúsítványhitelesítés tootest:**  

1. Az EAS-profil konfigurálása, amely eleget tesz a fenti követelmények hello hello alkalmazásban.  
2. Nyissa meg a hello alkalmazást, és győződjön meg arról, hogy mail szinkronizál. 

