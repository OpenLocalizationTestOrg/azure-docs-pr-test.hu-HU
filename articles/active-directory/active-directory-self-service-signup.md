---
title: "aaaWhat várja Azure önkiszolgáló regisztráció? | Microsoft Docs"
description: "Egy Azure-ban – áttekintés önkiszolgáló regisztrációt hogyan toomanage hello előfizetési folyamat, és hogyan tootake keresztül egy DNS-tartománynevet."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: dbf3b59e3807e98f7bf39f3d5591fcde01667323
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Mi az az Azure önkiszolgáló regisztráció?
Ez a témakör ismerteti a hello önkiszolgáló regisztrációt folyamatát, és hogyan tootake keresztül egy DNS-tartománynevet.  

## <a name="why-use-self-service-signup"></a>Miért érdemes használni önkiszolgáló regisztrációt?
* Get-ügyfelek tooservices cég gyorsabb.
* E-mail alapú ajánlatok szolgáltatás létrehozása.
* Hozzon létre előfizetési e-mail-alapú forgalom gyorsan azok könnyen megjegyezhető munkahelyi e-mail aliasok toocreate identitások engedélyezése a felhasználóknak.
* Nem felügyelt Azure-könyvtárak később a felügyelt könyvtárak tehető, és használja fel újra az egyéb szolgáltatásokat.

## <a name="terms-and-definitions"></a>Kifejezések és meghatározások
* **Önkiszolgáló regisztrációt**: Ez a felhasználó előfizet egy felhőalapú szolgáltatás és van egy automatikusan létrehozott számukra az Azure Active Directory (Azure AD) identitás alapján e-mail tartományukban hello módszer.
* **Nem felügyelt Azure-címtár**: Ez az identitásukat létrehozási helyének hello könyvtár. Egy nem felügyelt könyvtár értéke nem globális rendszergazda megegyező nevű könyvtárat.
* **E-mailek ellenőrzött felhasználói**: Ez egy felhasználói fiók típusa az Azure AD-ben. A felhasználó identitása önkiszolgáló szolgáltatásokat a regisztráció után automatikusan létrehozza az e-mailek ellenőrzött felhasználói néven ismert. Az e-mailek ellenőrzése felhasználó tagja rendszeres creationmethod címkéjű könyvtár = EmailVerified.

## <a name="user-experience"></a>Felhasználói élmény
Például tegyük fel, a felhasználó, akinek e-mail Dan@BellowsCollege.com érzékeny fájlok e-mailben kap. hello fájlok védetté tett az Azure Rights Management (Azure RMS). De Dan a szervezeten belül, csőrugó felsőoktatási rendelkezik nem regisztrált az Azure RMS, és nem rendelkezik telepített Active Directory RMS. Ebben az esetben Dan regisztrálhat egy ingyenes előfizetés tooRMS egyéni felhasználók számára a rendelés tooread hello védett fájlokat.

Ha Dan hello első felhasználó BellowsCollege.com toosign Ez az ajánlat önkiszolgáló feliratkozott az e-mail címmel, majd egy nem felügyelt directory létrejön BellowsCollege.com Azure AD-ben. Ha más felhasználók hello BellowsCollege.com tartományból a regisztrációt az ajánlat vagy egy hasonló önkiszolgáló ajánlat is rendelkeznek azonos a nem felügyelt hello létrehozott e-mail ellenőrzött felhasználói fiókok könyvtárhoz, az Azure-ban.

## <a name="admin-experience"></a>Rendszergazdai feladatok
Egy rendszergazda birtokló hello DNS-tartománynevét egy nem felügyelt Azure-címtár átveszi, vagy hello directory egyesítése után tulajdonjoga igazolására. hello a következő szakaszok ismertetik a hello rendszergazdai feladatok részletes, de összegzése:

* Egy nem felügyelt Azure-címtár átvételekor hello hello nem felügyelt címtár globális rendszergazdájának egyszerűen válik. Ez egy belső felvásárlási is nevezik.
* Egy nem felügyelt Azure-címtár egyesítése, hozzáadhat hello nem felügyelt directory tooyour felügyelt Azure-címtár hello DNS-tartománynevét, és a felhasználók-erőforrások leképezéseket jön létre, felhasználók továbbra is tooaccess szolgáltatások megszakítás nélkül. Egy külső felvásárlási is nevezik.

## <a name="what-gets-created-in-azure-active-directory"></a>Mi az Azure Active Directoryban jön létre?
#### <a name="directory"></a>Könyvtár
* Az Azure Active Directory címtár hello tartomány jön létre, egy címtárban tartományonként.
* hello Azure AD-címtárral rendelkezik nem globális rendszergazda segítségét.

#### <a name="users"></a>Felhasználók
* Minden felhasználó, aki regisztrál létrejön egy felhasználói objektum hello Azure AD-címtár.
* Minden felhasználói objektum a külső van megjelölve.
* Minden felhasználó kap, hogy regisztrált-e az toohello szolgáltatás.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Milyen jogcímek önkiszolgáló az Azure AD címtár saját tartomány?
Akkor igényelhet egy önkiszolgáló az Azure AD tartomány ellenőrzéséhez elvégzésével könyvtárába. Tartomány ellenőrzéséhez bizonyul, saját hello tartomány DNS-rekordok létrehozásával.

Két módon toodo egy DNS-felvásárlási az Azure AD-címtárhoz van:

* belső felvásárlási (Admin egy nem felügyelt Azure-címtár deríti fel, és azt akarja tooturn egy felügyelt könyvtárba)
* külső felvásárlási (Admin megpróbál egy új tartomány tootheir felügyelt az Azure directory tooadd)

Előfordulhat, hogy az érdekelt érvényesítése során a tartomány tulajdonosa, mert véve egy nem felügyelt directory keresztül a felhasználók végre önkiszolgáló regisztrációt, vagy előfordulhat, hogy egy új tartomány tooan meglévő felügyelt könyvtár hozzáadása után. Például egy contoso.com nevű tartomány, és azt szeretné, hogy tooadd contoso.co.uk vagy contoso.uk nevű.

## <a name="what-is-domain-takeover"></a>Mi az a tartomány felvásárlási?
A szakasz bemutatja hogyan toovalidate, hogy a tartomány tulajdonosa

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Mi az tartomány ellenőrzéséhez és miért van ezt fogja használni?
Rendelés tooperform műveleteihez a directory az Azure AD szükséges hello DNS-tartomány tulajdonjogát érvényesítése.  Érvényesítési hello tartomány lehetővé teszi, hogy Ön tooclaim hello könyvtár, és vagy előléptetéséhez hello önkiszolgáló directory tooa kezelt könyvtár, vagy egyesítési hello önkiszolgáló directory be egy meglévő könyvtár felügyelt.

## <a name="examples-of-domain-validation"></a>Példák tartomány ellenőrzéséhez
Nincsenek két módon toodo egy DNS-felvásárlási könyvtár:

* belső felvásárlási (például egy rendszergazda önkiszolgáló, nem felügyelt könyvtár deríti fel, és tooturn szeretne rendelni a felügyelt könyvtárban)
* külső felvásárlási (például egy rendszergazda egy új tartomány tooa felügyelt directory tooadd próbálkozik)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-toobe-a-managed-directory"></a>Belső felvásárlási - lépteti elő egy önkiszolgáló, nem felügyelt directory toobe kezelve
Ha így tesz, belső felvásárlási, hello directory alakítja, egy nem felügyelt directory tooa felügyelt könyvtárból. Toocomplete DNS-tartomány neve ellenőrzést, ahol az MX-rekord vagy létrehozhat egy TXT-rekord hello DNS-zónában van szüksége. A művelet:

* Ellenőrzi, hogy Ön a tulajdonosa hello tartomány
* Lehetővé teszi a felügyelt hello könyvtár
* Elérhetővé válnak, hello hello címtár globális rendszergazda

Tegyük fel, a rendszergazda a csőrugó felsőoktatási deríti fel, hogy hello iskolai a felhasználók regisztráltak-e önkiszolgáló ajánlatokról. Hello regisztrált hello DNS tulajdonosának neve BellowsCollege.com, hello rendszergazda érvényesítése hello DNS-neve az Azure-ban tulajdonjogát, és majd átveszi hello nem felügyelt directory. hello directory lesz kezelve, és hello rendszergazda van hello globális rendszergazdai szerepkörrel hello BellowsCollege.com könyvtár.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Külső felvásárlási - könyvtár önkiszolgáló egyesítése a létező felügyelt könyvtárat
Egy külső felvásárlási, a már van kezelve, és azt szeretné, hogy minden felhasználó és egy nem felügyelt directory toojoin directory kezelt csoportjait, nem pedig saját két külön könyvtárak.

Felügyelt könyvtár rendszergazdaként tartomány hozzáadása, és adott tartomány toohave vele társított egy nem felügyelt directory történik.

Például tegyük fel, a rendszergazda és contoso.com, egy tartomány nevét, amely regisztrált tooyour szervezet már rendelkezik egy felügyelt könyvtár. Azt tapasztalja, hogy a szervezet felhasználóinak végeztek el az önkiszolgáló bejelentkezési fel egy ajánlatot az e-mailek tartománynév használatával user@contoso.co.uk, amely egy másik, a szervezete tulajdonában lévő tartománynevet. Ezek a felhasználók fiókokat jelenleg rendelkeznek contoso.co.uk egy nem felügyelt könyvtárban.

Nem szeretné, hogy toomanage két külön könyvtárakban, ezért nem felügyelt könyvtár hello contoso.co.uk egyesítése a meglévő felügyelt informatikai directory contoso.com.

Külső felvásárlási követi DNS érvényesítési leírt eljárást belső felvásárlási hello.  A különbözetet: felhasználók és a szolgáltatások újratársított toohello informatikai kezelve.

#### <a name="whats-hello-impact-of-performing-an-external-takeover"></a>Mi az a hello hatása hajt végre egy külső felvásárlási?
Egy külső felvásárlási rendelkező felhasználók-erőforrások leképezéseket jön létre, így a felhasználók továbbra is tooaccess szolgáltatások megszakítás nélkül. Számos alkalmazás, beleértve az RMS egyéni felhasználók számára, kezelnek hello leképezési felhasználók-erőforrások is, és a felhasználók továbbra is tooaccess ezekbe a szolgáltatásokba módosítás nélkül. Ha egy alkalmazás nem kezeli a hello leképezési felhasználók-erőforrások hatékony, külső felvásárlási lehet gyenge élmény explicit módon letiltott tooprevent felhasználóit.

#### <a name="directory-takeover-support-by-service"></a>Directory felvásárlási támogatási szolgáltatás
Jelenleg a következő hello szolgáltatások támogatási felvásárlási:

* RMS

a következő szolgáltatások hello hamarosan támogatni felvásárlási:

* PowerBI

hello alábbi azonban nem, és szükséges további felügyeleti művelet toomigrate felhasználói adatok egy külső felvásárlási után.

* SharePoint/onedrive vállalati verzió

## <a name="how-tooperform-a-dns-domain-name-takeover"></a>Hogyan tooperform egy DNS-tartomány neve felvásárlási
Néhány lehetséges hogyan tooperform egy tartomány ellenőrzéséhez (és egy felvásárlási Ha):

1. Azure felügyeleti portál

   Olyan felvásárlási elindul egy tartomány hozzáadása végrehajtásával.  Ha egy könyvtár már létezik hello tartományhoz, akkor kell hello beállítás tooperform egy külső felvásárlási.

   Bejelentkezés toohello Azure-portálon a hitelesítő adatok használatával.  Keresse meg a meglévő directory tooyour, majd túl**tartomány hozzáadása**.
2. Office 365

   Hello beállításokat kínál a hello [tartományok kezelése](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) oldal az Office 365 toowork a tartományok és a DNS-rekordokat. Lásd: [ellenőrizze a tartományt, az Office 365-ben](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   a lépéseket követve hello szükséges tooperform egy Windows PowerShell használatával érvényesítési.

   | Lépés | A parancsmag toouse |
   | --- | --- |
   | A hitelesítő objektum létrehozása |A GET-Credential parancsmag |
   | TooAzure AD Connect |Connect-MsolService |
   | tartományok listájának beolvasása |Get-MsolDomain |
   | Feladat létrehozása |Get-MsolDomainVerificationDns |
   | DNS-rekord létrehozása |Ehhez a DNS-kiszolgálón |
   | Hello kérdés ellenőrzése |Confirm-MsolEmailVerifiedDomain |

Példa:

1. Csatlakozás tooAzure AD, melyeket használt toorespond toohello önkiszolgáló ajánlat hello hitelesítő adatokkal:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Tartományok listájának beolvasása:

    Get-MsolDomain
3. Ezután futtassa hello Get-MsolDomainVerificationDns parancsmag toocreate kihívást:

    Get-MsolDomainVerificationDns – tartománynév *your_domain_name* – mód DnsTxtRecord

    Példa:

    Get-MsolDomainVerificationDns – tartománynév contoso.com – mód DnsTxtRecord
4. Másolja, ez a parancs által visszaadott hello értékét (hello challenge).

    Példa:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9
5. A nyilvános DNS-névtérben hello előző lépésben másolt hello értéket tartalmazó DNS txt rekordot kell létrehozni.

    a rekord hello neve hello hello szülőtartomány nevét, ezért ez az erőforrásrekord hello DNS-szerepkör a Windows Server használatával hozzon létre, ha hello rekordnév üresen hagyja, és csak hello érték illessze be hello szövegmező
6. Hello megerősítése-MsolDomain parancsmag tooverify hello ellenőrző futtatása:

    Erősítse meg MsolEmailVerifiedDomain - tartománynév *your_domain_name*

    Példa:

    Erősítse meg MsolEmailVerifiedDomain - tartománynév contoso.com

Sikeres kihívást adja vissza toohello kérdés hiba nélkül.

## <a name="how-do-i-control-self-service-settings"></a>Hogyan szabályozza a önkiszolgáló beállítások?
Rendszergazdák két önkiszolgáló vezérlők ma rendelkezik. Szabályozhatja:

* -E a felhasználók kapcsolódhatnak hello directory e-mailben.
* E felhasználók licencelheti magukat az alkalmazások és szolgáltatások.

### <a name="how-can-i-control-these-capabilities"></a>Hogyan szabályozhatja ezeket a képességeket?
Egy rendszergazda konfigurálhatja ezeket az Azure AD Set-MsolCompanySettings parancsmag-paraméterek felületével:

* **AllowEmailVerifiedUsers** meghatározza, hogy a felhasználó létrehozása, vagy egy nem felügyelt directory join. A paraméter értéke túl$ hamis, nem a e-mailek ellenőrzése a felhasználók úgy csatlakozhatnak hello könyvtár.
* **AllowAdHocSubscriptions** regisztráljon tooperform önkiszolgáló felhasználók mostantól hello szabályozza. Ha a paraméter túl$ hamis, nem felhasználók hajthatnak végre önkiszolgáló regisztrációt.

### <a name="how-do-hello-controls-work-together"></a>Hogyan hello vezérlők működnek együtt?
A két paraméter használható együtt toodefine az önkiszolgáló pontosabb szabályozhatják regisztrálhat. Például a következő parancs hello lehetővé teszi felhasználók tooperform önkiszolgáló regisztrációt, de csak ha azoknak a felhasználóknak már rendelkezik fiókkal az Azure ad-ben (más szóval, felhasználók, akik egy e-mailek ellenőrzése fiók toobe, létre kell nem végezhető el az önkiszolgáló bejelentkezési fel):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

hello következő folyamatábra ismerteti összes hello különböző kombinációkban e paraméterek és hello eredményül kapott feltételek hello directory és az önkiszolgáló regisztrációt.

![][1]

További információt és példákat hogyan toouse ezeket a paramétereket: [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Lásd még:
* [Hogyan tooinstall Azure PowerShell és konfigurálása](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure parancsmag-referencia](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
