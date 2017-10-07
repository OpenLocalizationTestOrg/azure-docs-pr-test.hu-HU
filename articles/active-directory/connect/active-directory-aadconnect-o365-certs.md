---
title: "az Office 365 és az Azure AD felhasználók aaaCertificate megújítási |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooresolve érintő problémákat, amely egy tanúsítvány megújítására vonatkozó értesítést küldhet nekik e-mailek tooOffice 365 felhasználók."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b9b309e06949dc5488cd628650be413f366ed347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Office 365 és az Azure Active Directory összevonási tanúsítványok megújítása
## <a name="overview"></a>Áttekintés
Az Azure Active Directory (Azure AD) és Active Directory összevonási szolgáltatások (AD FS) közötti sikeres összevonáshoz hello tanúsítványok használják az AD FS toosign biztonsági jogkivonatokat tooAzure AD meg kell felelnie az Azure ad-ben konfigurált. Bármely eltérés toobroken megbízhatósági vezethet. Az Azure AD gondoskodik arról, hogy ezek az információk tárolódik szinkronban központi telepítésekor az AD FS és a webalkalmazás-Proxy (az extranetes hozzáféréshez).

Ez a cikk nyújt további információt toomanage a jogkivonat-aláíró tanúsítványokat, és szinkronban maradjanak az Azure ad-vel, a következő esetekben hello:

* Hello webalkalmazás-Proxy nem telepít, és ezért nem áll rendelkezésre az extranetes hello hello összevonási metaadatok.
* A jogkivonat-aláíró tanúsítványok hello alapértelmezett beállítása az AD FS nem használ.
* Egy külső identitásszolgáltatótól használ.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Az AD FS jogkivonat-aláíró tanúsítványok az alapértelmezett konfiguráció
hello jogkivonat-aláíró és jogkivonat-visszafejtési tanúsítványokat általában önaláírt tanúsítványokat, és egy évig helyes. Alapértelmezés szerint az AD FS tartalmaz egy automatikus megújítási nevezett folyamat **autocertificaterollover beállítást**. Ha az AD FS 2.0-s vagy újabb verzióját használja, Office 365 és az Azure AD automatikusan frissíti a tanúsítvány lejárata előtt.

### <a name="renewal-notification-from-hello-office-365-portal-or-an-email"></a>Megújítási hello Office 365 portál vagy e-mailben értesítést
> [!NOTE]
> Ha kapott e-mailben vagy a portál értesítései toorenew kéri fel a tanúsítvány az Office, lásd: [kezelése módosítja aláíró tanúsítványok tootoken](#managecerts) toocheck, ha bármilyen műveletet kell tootake. Microsoft lehetséges problémát okozhat a tanúsítvány megújításához küldi el, akkor is, ha nincs teendő toonotifications tisztában.
>
>

Az Azure AD kísérel meg toomonitor hello összevonási metaadatok és a frissítés hello jogkivonat-aláíró tanúsítványok, a metaadatok jelöli. hello jogkivonat-aláíró tanúsítványok, hello lejárta előtt 30 nappal az Azure AD ellenőrzi, ha új érhetők el tanúsítványok hello összevonási metaadatok lekérdezésével.

* Ha sikeresen kérdezze le a hello összevonási metaadatok és hello új tanúsítványok beolvasása, nem e-mailben értesítést vagy hello Office 365 portálon figyelmeztetés toohello felhasználói ad ki.
* Ha hello új jogkivonat-aláíró tanúsítványok nem olvashatók be, vagy hello összevonási metaadatok nem érhető el, vagy nincs engedélyezve az automatikus tanúsítványváltást, mert az Azure AD kibocsát e-mailben értesítést és hello Office 365 portálon figyelmeztetés.

![Az Office 365 portál értesítései](./media/active-directory-aadconnect-o365-certs/notification.png)

> [!IMPORTANT]
> AD FS-ben tooensure az üzletmenet folytonossága, rendszer használata esetén győződjön meg arról, hogy a kiszolgáló rendelkezik-e, hogy nem történik meg, az ismert problémák a hitelesítési hibák száma a következő frissítések hello. Ez csökkenti az ismert AD FS proxy server kapcsolatos problémát a megújítási és későbbi megújítási időszakok:
>
> Server 2012 R2 - [Windows Server előfordulhat, hogy 2014 összegzése](http://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 és a 2012 - [proxyn keresztül történő hitelesítés nem sikerül, a Windows Server 2012 vagy Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)
>
>

## Ellenőrizze, hello tanúsítványok toobe frissítése<a name="managecerts"></a>
### <a name="step-1-check-hello-autocertificaterollover-state"></a>1. lépés: Ellenőrizze a hello autocertificaterollover beállítást
Az AD FS kiszolgálón nyissa meg a Powershellt. Ellenőrizze, hogy hello autocertificaterollover beállítást értéke tooTrue.

    Get-Adfsproperties

![Autocertificaterollover beállítást](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

>[!NOTE] 
>Ha az AD FS 2.0 használ, előbb futtassa az Add-Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>2. lépés: Győződjön meg róla, hogy az AD FS és az Azure AD Szinkronizáló
Az AD FS-kiszolgálón nyisson meg hello Azure AD PowerShell-parancssorba, és csatlakozzon tooAzure AD.

> [!NOTE]
> Letöltheti az Azure AD PowerShell [Itt](https://technet.microsoft.com/library/jj151815.aspx).
>
>

    Connect-MsolService

Ellenőrizze a hello tanúsítványok az AD FS-ben, és az Azure AD megbízhatósági tulajdonságainak hello tartomány.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Ha mindkét hello ujjlenyomatok hello kimenetek egyezik, a tanúsítványok szinkronban az Azure AD-val.

### <a name="step-3-check-if-your-certificate-is-about-tooexpire"></a>3. lépés: Annak ellenőrzése, hogy a tanúsítvány tooexpire kapcsolatban
A Get-MsolFederationProperty vagy a Get-AdfsCertificate hello kimenete ellenőrizze a hello az időpontra a "Nem után." Ha hello dátum 30 napon belül számítógépnél, kell hajtsa végre a műveletet.

| Autocertificaterollover beállítást | Az Azure ad-vel szinkronizált tanúsítványok | Az összevonási metaadatok nyilvánosan elérhető | Érvényesség | Műveletek |
|:---:|:---:|:---:|:---:|:---:|
| Igen |Igen |Igen |- |Nincs szükség művelet végrehajtására. Lásd: [automatikusan megújítási jogkivonat-aláíró tanúsítvány](#autorenew). |
| Igen |Nem |- |Kevesebb, mint 15 nappal |Azonnal újítsa meg. Lásd: [manuális megújítása jogkivonat-aláíró tanúsítvány](#manualrenew). |
| Nem |- |- |30 napon belül |Azonnal újítsa meg. Lásd: [manuális megújítása jogkivonat-aláíró tanúsítvány](#manualrenew). |

\[Nem számít,-]

## Aláíró tanúsítvány automatikusan hello-token megújításához (ajánlott)<a name="autorenew"></a>
Nincs szükség tooperform bármely manuális módszerrel is hello következő teljesülése esetén:

* A webalkalmazás-Proxy, amely engedélyezheti a hozzáférést toohello összevonási metaadatok extranetes hello telepített.
* Az AD FS hello alapértelmezett konfigurációt (tehát az autocertificaterollover beállítást engedélyezve van) használ.

Ellenőrizze, hogy a tanúsítvány hello tooconfirm következő hello automatikusan frissíthető.

**1. hello AD FS tulajdonság autocertificaterollover beállítást tooTrue be kell állítani.** Ez azt jelzi, hogy az AD FS automatikusan hoz létre új jogkivonat-aláíró és jogkivonat-visszafejtési tanúsítványok előtt hello régi kiépítettektől eltérő lejár.

**2. hello AD FS összevonási metaadatok nyilvánosan elérhető.** Ellenőrizze, hogy az összevonási metaadatok nyilvánosan elérhető toohello navigálva követően URL-címet egy olyan számítógépről hello nyilvános internet (Kijelentkezés hello vállalati hálózaton):

https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

Ha `(your_FS_name) `hello összevonási szolgáltatás állomásneve a szervezet használja, például: fs.contoso.com helyére.  Ha mindkét képes tooverify beállítások sikeresen, nem rendelkezik toodo semmi más.  

Példa: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Hello jogkivonat-aláíró manuálisan a tanúsítvány megújítása<a name="manualrenew"></a>
Úgy is dönthet, toorenew hello jogkivonat-aláíró tanúsítványok manuálisan. Például hello következő esetekben előfordulhat, hogy jobban használható manuális megújítási:

* Jogkivonat-aláíró tanúsítványok nem önaláírt tanúsítványokat is. hello leggyakoribb ennek oka, hogy a szervezet kezeli-e az AD FS-tanúsítványok egy szervezeti hitelesítésszolgáltatótól származó regisztrálva.
* Hálózati biztonság nem engedélyezi a hello összevonási metaadatok toobe nyilvánosan elérhető.

Ezekben az esetekben minden egyes hello jogkivonat-aláíró tanúsítványok frissítése is frissítenie kell az Office 365 tartomány hello PowerShell-paranccsal, frissítés-MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>1. lépés: Győződjön meg arról, hogy az AD FS rendelkezik-e az új jogkivonat-aláíró tanúsítványok
**Nem alapértelmezett konfigurációja**

Ha az AD FS nem alapértelmezett konfigurálása (Ha **autocertificaterollover beállítást** értéke túl**hamis**), valószínűleg használ (nem önaláírt) egyéni tanúsítványokat. További információ a hogyan toorenew hello AD FS jogkivonat-aláíró tanúsítványok: [az ügyfelek nem használják az AD FS önaláírt tanúsítványok](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Az összevonási metaadatok nincs nyilvánosan elérhető**

A hello, ugyanakkor, ha **autocertificaterollover beállítást** értéke túl**igaz**, de az összevonási metaadatok nem nyilvánosan elérhető, először győződjön meg arról, hogy új jogkivonat-aláíró tanúsítványok AD hozták létre FS. Győződjön meg arról, még az új jogkivonat-aláíró tanúsítványok által véve hello a következő lépéseket:

1. Győződjön meg arról, hogy van-e bejelentkezve toohello elsődleges AD FS-kiszolgálón.
2. Ellenőrizze a hello aktuális aláírási tanúsítványok az AD FS nyisson meg egy PowerShell-parancsablakot, és fut a következő parancs hello:

    PS C:\>Get-ADFSCertificate – CertificateType jogkivonat-aláíró

   > [!NOTE]
   > Ha az AD FS 2.0 használ, először Add-Pssnapin Microsoft.Adfs.Powershell kell futtatni.
   >
   >
3. Tekintse meg hello parancs kimenetében, a felsorolt tanúsítványok. Az AD FS generált új tanúsítványt, ha két tanúsítványt hello kimenet láthatja: egy a mely hello **IsPrimary** értéke **igaz** és hello **NotAfter** dátum 5 nap, és amelyek egy belül **IsPrimary** van **hamis** és **NotAfter** jövőbeli hello év tárgya.
4. Ha csak egy tanúsítvány tekintse meg, és hello **NotAfter** 5 napon belül dátum, akkor kell toogenerate egy új tanúsítványt.
5. toogenerate egy új tanúsítvány hajtható végre a következő parancsot egy PowerShell-parancs parancssorba hello: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Hello frissítés ellenőrizze hello újra a következő parancs futtatásával: PS C:\>Get-ADFSCertificate – CertificateType jogkivonat-aláíró

Most már két tanúsítványt kell szerepelnie, amely az egyik van egy **NotAfter** dátuma jövőbeli hello, és melyik hello körülbelül egy év **IsPrimary** értéke **hamis**.

### <a name="step-2-update-hello-new-token-signing-certificates-for-hello-office-365-trust"></a>2. lépés: Hello új jogkivonat-aláíró tanúsítványok hello Office 365 bizalmi kapcsolat frissítése
Office 365 frissítése hello új jogkivonatot aláíró tanúsítványok toobe hello a megbízhatósági kapcsolatban, az alábbiak szerint használható.

1. Nyissa meg a Microsoft Active Directory modul Windows Powershellhez készült Azure hello.
2. Futtassa a $cred = Get-Credential. Ha ez a parancsmag felszólítja a hitelesítő adatokat, írja be a felhőalapú szolgáltatás rendszergazdai fiók hitelesítő adatait.
3. Futtassa a Connect-MsolService – hitelesítőadat-$cred. Ez a parancsmag létrehozza a toohello felhőalapú szolgáltatás. Olyan környezetben, amely csatlakoztatja toohello felhőalapú szolgáltatás létrehozása előtt meg kell adni futtató hello hello eszköz által telepített további parancsmagokat.
4. Ha ezek a parancsok futtat egy számítógépen, amely nincs hello AD FS elsődleges összevonási kiszolgálón, futtassa a Set-MSOLAdfscontext-számítógép <AD FS primary server>, ahol <AD FS primary server> hello hello elsődleges AD FS-kiszolgáló belső tartománynév van. Ez a parancsmag olyan környezetben, amely csatlakoztatja tooAD FS hoz létre.
5. Futtassa az Update-MSOLFederatedDomain – tartománynév <domain>. Ez a parancsmag frissíti az AD FS beállításokat hello hello felhőalapú szolgáltatás, és konfigurálja a hello megbízhatósági kapcsolat a két hello között.

> [!NOTE]
> Ha több felső szintű tartomány, például a contoso.com és fabrikam.com, kell toosupport használnia kell a hello **SupportMultipleDomain** bármely parancsmagok kapcsoló. További információkért lásd: [több felső szintű tartomány támogatása](active-directory-aadconnect-multiple-domains.md).
>
>

## Azure AD-megbízhatóság javítása az Azure AD Connect használatával<a name="connectrenew"></a>
Ha konfigurálta az AD FS-farm és az Azure AD-megbízhatóság az Azure AD Connect használatával, használhatja az Azure AD Connect toodetect, ha szüksége tootake semmilyen művelet a jogkivonat-aláíró tanúsítványok. Ha toorenew hello tanúsítványokra van szükség, így használhatja az Azure AD Connect toodo.

További információkért lásd: [hello megbízhatóság javítása](active-directory-aadconnect-federation-management.md).
