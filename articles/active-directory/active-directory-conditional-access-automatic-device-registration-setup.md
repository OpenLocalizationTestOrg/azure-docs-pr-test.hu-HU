---
title: "aaaHow tooconfigure automatikus regisztrálása az Azure Active Directoryval Windows tartományhoz csatlakozó eszközök |} Microsoft Docs"
description: "Állítsa be a tartományhoz csatlakoztatott Windows-eszközök tooregister automatikusan és értesítések nélkül történik az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 6a1aab753f5456ed06ba7979ab05f70f29b4ddee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Hogyan tooconfigure az automatikus regisztráció Windows tartományhoz csatlakoztatott eszközökre az Azure Active Directoryval

toouse [Azure Active Directory eszközalapú feltételes hozzáférési](active-directory-conditional-access-azure-portal.md), a számítógépek regisztrálni kell az Azure Active Directoryval (Azure AD). Hello segítségével a szervezet regisztrált eszközöket is elérhetővé [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) hello parancsmag [Azure Active Directory PowerShell-modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0). 

Ez a cikk nyújt hello lépéseket a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása hello konfigurálása az Azure AD a szervezetében.


További információ:

- Feltételes hozzáférés, lásd: [Azure Active Directory eszközalapú feltételes hozzáférési](active-directory-conditional-access-azure-portal.md). 
- Windows 10-eszközök hello munkahelyi és fokozott hello lép, amikor regisztrál az Azure ad-vel, lásd: [hello Enterprise Windows 10: eszközök használata munkára](active-directory-azureadjoin-windows10-devices-overview.md).
- Windows 10 nagyvállalati E3 csomag a CSP-hez, lásd: hello [CSP áttekintése a Windows 10 nagyvállalati E3 csomag](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).


## <a name="before-you-begin"></a>Előkészületek

Mielőtt elkezdené a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása hello konfigurálása a környezetben, tanulmányozza át hello támogatott forgatókönyvek és hello korlátozásokkal.  

tooimprove hello olvashatóságát hello leírásokat, ez a témakör a következő kifejezés hello használja: 

- **Aktuális Windows-eszközök** -e kifejezés toodomain csatlakoztatott eszközök a Windows 10 vagy Windows Server 2016 rendszerű.
- **Windows-kezelés régebbi eszközök** -e kifejezés tooall **támogatott** tartományhoz csatlakoztatott Windows-eszközök végrehajtott futó Windows 10 és Windows Server 2016.  


### <a name="windows-current-devices"></a>Aktuális Windows-eszközök

- Hello Windows asztali operációs rendszert futtató eszközök esetén ajánlott a Windows 10 évforduló frissítést (verzió: 1607) vagy újabb. 
- aktuális Windows-eszközök regisztrációja hello **van** nem összevont környezetekben, például a jelszó kivonatát szinkronizálási konfiguráció támogatott.  


### <a name="windows-down-level-devices"></a>Régebbi Windows-eszközök

- a következő Windows-kezelés régebbi eszközök hello támogatottak:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- Windows-kezelés régebbi eszközök regisztrációja hello **van** keresztül zökkenőmentes egyszeri bejelentkezés nem összevont környezetekben támogatott [Azure Active Directory zökkenőmentes egyszeri bejelentkezést](https://aka.ms/hybrid/sso).
- Windows-kezelés régebbi eszközök regisztrációja hello **nem** eszközök központi profilok használata támogatott. Ha a központi profilok vagy a beállítások, Windows 10 használata biztosítja.



## <a name="prerequisites"></a>Előfeltételek

Mielőtt engedélyezése hello automatikus regisztráció tartományhoz csatlakoztatott eszközök a szervezetben, szüksége van-e toomake, hogy az Azure AD legfrissebb változata fut-e csatlakozni.

Az Azure AD Connect:

- A helyszíni Active Directory (AD) és a hello eszközobjektumot az Azure AD hello számítógépfiók hello társítását tartja. 
- Lehetővé teszi, hogy más eszköz kapcsolódó szolgáltatások, például a vállalati Windows Hello.



## <a name="configuration-steps"></a>Konfigurációs lépések

Ez a témakör minden tipikus forgatókönyve hello szükséges lépéseket tartalmazza.  
A következő tábla tooget áttekintést hello a forgatókönyvhöz szükséges hello használata:  



| Lépések                                      | A Windows jelenlegi és a jelszó Jelszókivonat-szinkronizálás | A Windows jelenlegi és a következő összevonáshoz | Windows-kezelés régebbi |
| :--                                        | :-:                                    | :-:                            | :-:                |
| 1. lépés: A szolgáltatáskapcsolódási pont konfigurálása | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| 2. lépés: A jogcímek kiállítási beállítása           |                                        | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| 3. lépés: A Windows 10-eszközök engedélyezése      |                                        |                                | ![Jelölőnégyzet][1]        |
| 4. lépés: Központi telepítési és a bevezetés ellenőrzése     | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| 5. lépés: A regisztrált eszközök ellenőrzése          | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |



## <a name="step-1-configure-service-connection-point"></a>1. lépés: A szolgáltatáskapcsolódási pont konfigurálása

hello szolgáltatás kapcsolódási pontjának (SCP) objektum hello regisztrációs toodiscover információkat az Azure AD bérlő során használatos az eszközök által. A helyszíni Active Directoryban (AD) hello SCP objektumot hello automatikus regisztráció a tartományhoz csatlakozó eszközök hello konfigurációs névhasználati környezet partíció hello számítógép erdő léteznie kell. Csak egy konfigurációs névhasználati környezet minden erdőre van. Többerdős Active Directory-konfiguráció esetén a hello szolgáltatáskapcsolódási pont léteznie kell az összes olyan erdőben, a tartományhoz csatlakoztatott számítógépeket tartalmazó.

Használhatja a hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) parancsmag tooretrieve hello konfigurációelnevezési környezetében az erdőben.  

Active Directory-tartomány nevét hello erdő *fabrikam.com*, hello konfigurációelnevezési környezetében van:

`CN=Configuration,DC=fabrikam,DC=com`

Az erdő hello SCP objektum hello automatikus regisztráció a tartományhoz csatlakoztatott eszközök a következő helyen található:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Attól függően, hogy telepítette az Azure AD Connect hello SCP objektum lehet, hogy már meg vannak adva.
Hello objektum hello létezésének ellenőrzése, és használja a következő Windows PowerShell-parancsfájl hello hello felderítési értékek lekérését: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

Hello **$scp. Kulcsszavak** az alábbiakat mutatja be hello Azure AD bérlői kapcsolatos információkat, például:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Ha hello szolgáltatáskapcsolódási pont nem létezik, létrehozhatja hello futtatásával `Initialize-ADSyncDomainJoinedComputerSync` parancsmag az Azure AD Connect-kiszolgálón. Vállalati rendszergazda hitelesítő adatai szükséges toorun van ennek a parancsmagnak.  
hello parancsmagot:

- Hello Active Directory-erdő az Azure AD Connect csatlakozik-e hello szolgáltatáskapcsolódási pontot hoz létre. 
- Toospecify hello használatához `AdConnectorAccount` paraméter. Ez az hello fiók, amely az Active Directory connector fiók az Azure AD connect van konfigurálva. 


hello következő parancsfájlt mutat egy példát a hello parancsmag használatával. Ezt a parancsfájlt a `$aadAdminCred = Get-Credential` tootype felhasználónév megadása szükséges. Tooprovide hello felhasználónév hello felhasználó egyszerű felhasználónév (UPN) formátumban van szüksége (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

Hello `Initialize-ADSyncDomainJoinedComputerSync` parancsmagot:

- Hello Active Directory PowerShell-modul, a tartományvezérlőn futó Active Directory webszolgáltatások használja. Az Active Directory webszolgáltatások a Windows Server 2008 R2 rendszerű tartományvezérlők és újabb rendszer.
- Csak akkor hello támogatott **MSOnline PowerShell modul verziója 1.1.166.0**. toodownload Ez a modul ezt [hivatkozás](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).   

A Windows Server 2008 vagy korábbi verzióit futtató tartományvezérlők parancsfájllal hello toocreate hello szolgáltatáskapcsolódási pont alatt.

Egy Többerdős konfigurációs parancsfájl toocreate hello szolgáltatáskapcsolódási pont minden olyan erdőben, ahol a számítógépek létezik a következő hello kell használnia:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a>2. lépés: A jogcímek kiállítási beállítása

Az összevont Azure Active Directory beállítása, a eszközök támaszkodnak az Active Directory összevonási szolgáltatások (AD FS) vagy 3. fél a helyi összevonási szolgáltatás tooauthenticate tooAzure AD. Eszközök hitelesítéséhez tooget egy hozzáférési jogkivonat tooregister hello Azure Active Directory Eszközregisztrációs szolgáltatás (Azure DRS) ellen.

A Windows jelenlegi eszközök hitelesítés integrált Windows-hitelesítés tooan aktív WS-Trust végpont (1.3 vagy 2005 verzió) hello a helyi összevonási szolgáltatás által üzemeltetett használatával.

> [!NOTE]
> AD FS-ben vagy használatakor **adfs/services/megbízhatósági/13/windowstransport** vagy **adfs/services/megbízhatósági/2005/windowstransport** engedélyezve kell lennie. Ha hello hitelesítési Webproxyt használ, bizonyosodjon meg arról, hogy a végpont hello proxy használatával van közzétéve. Láthatja, hogy milyen végpontjainak engedélyezve vannak – hello AD FS felügyeleti konzolon a **szolgáltatás > végpontok**.
>
>Ha a helyi összevonási szolgáltatás AD FS nincs telepítve, lépésekkel hello a szállító toomake meg arról, hogy a WS-Trust 1.3 vagy 2005 végpontjainak és, hogy ezek hello metaadatok Exchange fájl (MEX) keresztül közzétett támogatják a.

hello következő jogcímeket léteznie kell az eszköz regisztrációja toocomplete Azure DRS által kapott hello jogkivonat. Azure DRS egy eszközobjektumot hoz létre az egyes ezeket az információkat, majd az Azure AD Connect tooassociate az újonnan létrehozott hello eszközobjektum hello számítógép fiók helyszíni által használt Azure AD.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Ha több ellenőrzött tartomány nevét, a következő számítógépek jogcím tooprovide hello szüksége:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Ha egy ImmutableID jogcímet (pl. másodlagos bejelentkezési Azonosítóval) kiadása már meg van szüksége egy megfelelő jogcím tooprovide számítógépek:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

A következő részekben hello akkor talál információkat:
 
- az egyes jogcímek hello értékeket kell rendelkeznie
- Hogyan definíció jelenne meg az AD FS-ben

hello definition segít tooverify e hello értékek jelen-e, vagy ha szüksége van-e toocreate őket.

> [!NOTE]
> Ha az AD FS nem adja meg a helyi összevonási kiszolgáló, kövesse a gyártói utasításokat toocreate hello megfelelő konfiguráció tooissue ezeket a jogcímeket.

### <a name="issue-account-type-claim"></a>A probléma fiók típusa jogcím

**`http://schemas.microsoft.com/ws/2012/01/accounttype`**– Ezt az igényt az értéket kell tartalmaznia, **DJ**, amely azonosítja a hello eszközt egy tartományhoz csatlakozó számítógép. Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a>A probléma objectGUID hello számítógép fiók helyszínen

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-A jogcím tartalmaznia kell a hello **objectGUID** hello értékének a helyi számítógépfiókot. Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a>A probléma objectSID hello számítógép fiók helyszínen

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-A jogcím tartalmaznia kell a hello hello **objectSid** hello értékének a helyi számítógépfiókot. Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Számítógép issuerID kibocsátani, ha több tartománynév ellenőrzése az Azure ad-ben

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-A jogcím hello egységes erőforrás-azonosító (URI) bármely hello tartományneveket, amelyekben hello csatlakozzon a helyi összevonási szolgáltatás (AD FS vagy 3. fél) kibocsátó hello token ellenőrizni kell tartalmaznia. Az AD FS-ben hello azokat, az alábbi abban, hogy a megadott sorrendben után azokat a fenti hello nézzenek kiállítás-transzformációs szabályok is hozzáadhat. A felhasználók szükséges vegye figyelembe, hogy egy tooexplicitly probléma hello szabály. Az alábbi hello szabályok és a számítógép-hitelesítés felhasználói azonosító első szabály jelenik meg.

    @RuleName = "Issue account type with hello value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


A fenti hello jogcímek

- `$<domain>`hello AD FS szolgáltatás URL-címe
- `<verified-domain-name>`a szükséges tooreplace egyik a ellenőrzött tartomány nevét az Azure AD helyőrző



Ellenőrzött tartomány nevét kapcsolatos további tudnivalókért lásd: [hozzáadása egy egyéni tartomány nevét az Active Directory tooAzure](active-directory-add-domain.md).  
a vállalat ellenőrzött tartományok listájának tooget, hello használható [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) parancsmag. 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Számítógép ImmutableID ki, ha egy, a felhasználók létező (pl. másodlagos bejelentkezési azonosító beállítása)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-A jogcím számítógépek érvényes értéket kell tartalmaznia. Az AD FS-ben is létrehozhat egy kiadási átalakítási szabálykészlet bocsát az alábbiak szerint:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a>Segítő parancsfájl toocreate hello AD FS kiadás átalakítási szabályai

hello következő parancsfájl segít azzal hello létrehozott hello kiállítási átalakítási szabályok a fent leírt.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with hello value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Megjegyzések 

- Ez a parancsfájl hello szabályok toohello meglévő szabály fűzi hozzá. Nem futnak hello parancsfájl kétszer mivel a szabályok készletét hello volna hozzá kétszer. Győződjön meg arról, hogy nem megfelelő szabályok tartoznak (körülmények hello megfelelő) jogcímek hello parancsfájl ismételt futtatása előtt.

- Ha több ellenőrzött tartomány nevét (ahogy hello Azure AD portálon vagy hello Get-MsolDomains parancsmag segítségével), állítsa hello **$multipleVerifiedDomainNames** hello a parancsfájl-túl**$true**. Győződjön meg arról is, hogy távolítsa el a meglévő issuerid jogcím létrehozott előfordulhat, hogy az Azure AD Connect vagy más eszközök segítségével. Íme egy példa a ehhez a szabályhoz:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Ha már kiadott egy **ImmutableID** vonatkozó felhasználói fiókok, állítsa be hello **$immutableIDAlreadyIssuedforUsers** hello a parancsfájl-túl**$true**.

## <a name="step-3-enable-windows-down-level-devices"></a>3. lépés: Engedélyezze a régebbi Windows-eszközök

Ha néhány, a tartományhoz csatlakoztatott eszközök a Windows-kezelés régebbi eszközöket, akkor:

- Házirend beállítása az Azure AD tooenable felhasználók tooregister eszközök.
 
- Konfigurálja a helyi összevonási szolgáltatás tooissue jogcímek toosupport **integrált Windows-hitelesítéssel (IWA)** az eszközregisztrációhoz tartozó.
 
- Adja hozzá a hello Azure AD eszköz hitelesítési végpont toohello helyi intranetes zóna tooavoid tanúsítványt kér hello eszköz hitelesítéséhez.

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a>Az Azure AD tooenable házirend beállítása felhasználói tooregister eszköz

tooregister Windows régebbi eszközöket kell toomake meg arról, hogy hello tooallow felhasználók tooregister eszközök beállítása az Azure AD-be van állítva. A hello Azure-portálon a beállítás található:

`Azure Active Directory > Users and groups > Device settings`
    
hello következő házirendet be kell állítani túl**összes**: **felhasználók előfordulhat, hogy regisztrálják az eszközeiket az Azure ad szolgáltatással**

![Eszközök regisztrálása](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a>A helyi összevonási szolgáltatás konfigurálása 

A helyi összevonási szolgáltatás támogatnia kell a kiállító hello **authenticationmehod** és **wiaormultiauthn** jogcímek fogadására egy hitelesítési kérelmek betöltő függő fél toohello az Azure AD egy resouce_params paraméterhez a kódolt érték látható:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Ha ilyen kérést, hello a helyi összevonási szolgáltatás hello felhasználói integrált Windows-hitelesítésen keresztül kell hitelesítenie, és akkor sikeres, a következő két jogcímek hello kiadja:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

Az AD FS-ben hozzá kell adnia egy kiadási átalakítási szabálykészlet adott folyamaton keresztül hello hitelesítési módszert.  

**tooadd Ez a szabály:**

1. A hello AD FS felügyeleti konzolon váltson túl`AD FS > Trust Relationships > Relying Party Trusts`.
2. Kattintson a jobb gombbal a hello Microsoft Office 365 Identitásplatformmal függő entitás megbízhatósági objektum, majd válassza ki **Jogcímszabályok szerkesztése**.
3. A hello **kiadás átalakítási szabályai** lapon jelölje be **szabály hozzáadása**.
4. A hello **jogcímszabály** sablon listáról válassza ki **jogcímek küldése egyéni szabály segítségével**.
5. Válassza ki **következő**.
6. A hello **Jogcímszabály nevének** mezőbe írja be **hitelesítési módszer Jogcímszabály**.
7. A hello **jogcímszabály** mezőbe, a következő szabály típusa hello:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Az összevonási kiszolgálón, írja be az alábbi PowerShell-paranccsal hello cseréje után  **\<RPObjectName\>**  nevű hello függő entitás objektum az az Azure AD függő entitás megbízhatósági objektum. Ez az objektum neve általában **Microsoft Office 365 Identitásplatformmal**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a>Hello Azure AD eszköz hitelesítési végpont toohello Helyi Intranet zóna hozzáadása

tooavoid tanúsítvány kérni fogja a felhasználók az eszközök regisztrálása a hitelesítéshez tooAzure AD tolható ki a házirend tooyour tartományhoz csatlakozó eszközök tooadd hello a következő URL-cím toohello helyi intranetzónához az Internet Explorerben:

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a>4. lépés: Központi telepítési és a bevezetés ellenőrzése

Amikor befejezte a hello szükséges lépéseket, tartományhoz csatlakozó eszközök készen tooautomatically regisztrálása az Azure ad-val. Minden tartományhoz csatlakozó eszközök a Windows 10 évforduló Update és Windows Server 2016 rendszert futtató automatikusan regisztrálja az Azure AD-eszközön indítsa újra, vagy felhasználói bejelentkezés. Új eszközök regisztrálása az Azure AD, ha hello eszköz újraindul hello tartomány illesztési művelet befejeződése után.

Olyan eszközök, amelyek túl volt korábban munkahelyhez tooAzure AD (például az Intune-hoz) átmenet"*tartományhoz csatlakoztatott, aad-ben regisztrált*"; azonban csak bizonyos idő az a folyamat toocomplete toohello normál miatt az összes eszközön tartomány- és felhasználói tevékenységek áramló.

### <a name="remarks"></a>Megjegyzések

- A csoportházirend objektumot toocontrol hello Bevezetés a Windows Server 2016 tartományhoz csatlakozó számítógépek és Windows 10 automatikus regisztráció is használhatja.

- Windows 10 2015. November automatikus frissítés regiszterekben az Azure ad-val **csak** Ha hello bevezetés csoportházirend-objektum be van állítva.

- toorollout hello az automatikus regisztráció a Windows-kezelés régebbi rendszerű számítógépek, telepíthet egy [Windows Installer-csomag](#windows-installer-packages-for-non-windows-10-computers) toocomputers választott.

- Ha ügyfélleküldéses hello csoportházirend objektum tooWindows 8.1 tartományhoz csatlakozó eszközök, regisztrációs megpróbálkozik az erdőfelderítéssel; azt ajánljuk, hogy használja-e hello [Windows Installer-csomag](#windows-installer-packages-for-non-windows-10-computers) tooregister összes Windows-kezelés régebbi rendszerű eszközre. 

### <a name="create-a-group-policy-object"></a>A csoportházirend-objektum létrehozása 

a jelenlegi Windows-számítógép az automatikus regisztráció toocontrol hello bevezetésének, telepítsen hello **eszközként regisztrálja a tartományhoz csatlakozó számítógépek** csoportházirend-objektum toohello eszközök tooregister szeretné. Telepíthet például hello házirend tooan szervezeti egység vagy tooa biztonsági csoport.

**tooset hello házirend:**

1. Nyissa meg **Kiszolgálókezelő**, és folytassa a túl`Tools > Group Policy Management`.
2. Nyissa meg, amely megfelel a toohello tartományba, ahol tooactivate automatikus regisztráció a jelenlegi Windows-számítógépek toohello tartományi csomópontot.
3. Kattintson a jobb gombbal **csoportházirend-objektumok**, majd válassza ki **új**.
4. Írja be a csoportházirend-objektum nevét. Például *az automatikus regisztráció tooAzure AD*. Kattintson az **OK** gombra.
5. Kattintson a jobb gombbal az új csoportházirend-objektumot, majd válassza ki **szerkesztése**.
6. Nyissa meg túl**számítógép konfigurációja** > **házirendek** > **felügyeleti sablonok** > **Windows Összetevők** > **Eszközregisztráció**. Kattintson a jobb gombbal **eszközként regisztrálja a tartományhoz csatlakozó számítógépek**, majd válassza ki **szerkesztése**.
   
   > [!NOTE]
   > Ez a csoportházirend-sablon hello Csoportházirend kezelése konzol korábbi verzióiból át lett nevezve. Egy korábbi hello konzol használatakor lépjen túl`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`. 

7. Válassza ki **engedélyezve**, majd válassza ki **alkalmaz**.
8. Kattintson az **OK** gombra.
9. Hivatkozás hello csoportházirend objektum tooa tetszőleges helyre. Például társíthatja az adott szervezeti egység tooa. Is sikertelen csatolás tooa meghatározott biztonsági számítógépek csoportja, amelyek automatikusan regisztrálja az Azure ad-val. tooset minden tartományhoz csatlakoztatott Windows 10 és Windows Server 2016 a szervezet számítógépeire, hivatkozás hello csoportházirend objektum toohello tartományi házirendet.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>A Windows 10 számítógépek Windows Installer-csomag

tooregister tartományhoz csatlakoztatott Windows-kezelés régebbi számítógépek összevont környezetben, töltse le és telepítse a Windows Installer-csomag (.msi) a letöltőközpontból: hello [Microsoft munkahelyi csatlakoztatás Windows 10 számítógépek](https://www.microsoft.com/en-us/download/details.aspx?id=53554) lap.

Hello csomagot a szoftverek terjesztési rendszer például System Center Configuration Manager segítségével telepíthet. hello csomag támogatja hello szabványos beavatkozás nélküli telepítés beállításainak hello *csendes* paraméter. A System Center Configuration Manager aktuális ágának további előnyökkel jár a korábbi verziók, például hello képes végrehajtani tootrack regisztráció. További információkért lásd: [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

hello telepítő ütemezett feladatot hoz létre hello felhasználó környezetében futó hello rendszeren. hello feladat indítása tooWindows hello felhasználó bejelentkezésekor. hello feladat csendes regisztrálja hello eszköz hitelesítése az integrált Windows-hitelesítést használó után hello felhasználói hitelesítő adatokat az Azure AD-val. toosee hello ütemezett feladat, hello eszközön Ugrás túl**Microsoft** > **munkahelyi csatlakoztatás**, és folytassa a toohello Feladatütemező könyvtár.

## <a name="step-5-verify-registered-devices"></a>5. lépés: A regisztrált eszközök ellenőrzése

Ellenőrizheti a sikeres regisztrált eszközöket a szervezet hello segítségével [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) hello parancsmag [Azure Active Directory PowerShell-modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Ez a parancsmag kimenete hello az Azure AD-ben regisztrált eszközök jeleníti meg. tooget minden eszköz használata hello **-összes** paramétert, majd a szűrő őket hello segítségével **deviceTrustType** tulajdonság. Tartományhoz csatlakozó eszközök értéket veheti fel **tartományhoz**.

## <a name="next-steps"></a>Következő lépések

* [Automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md)
* [Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md)
* [Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10](active-directory-device-registration-troubleshoot-windows-legacy.md)
* [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
