---
title: "aaaWhat az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban? | Microsoft Docs"
description: "Azure Active Directory tooenable egyszeri bejelentkezés tooall hello SaaS és a webes alkalmazások, amelyekre szüksége van a vállalati használja."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 75d1a3fd-b3c5-4495-a5c8-c4c24145ff00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curyand
ms.openlocfilehash: 429522cbd570ab27359c4630c5a6d7b25b692ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?
Egyszeri bejelentkezés azt jelenti, hogy képes tooaccess alatt álló összes hello alkalmazások és erőforrások toodo üzleti szüksége a bejelentkezéssel csak egyszer egyetlen felhasználói fiókkal. Miután bejelentkezett, van-e hozzáférési összes hello alkalmazás anélkül, hogy a szükséges tooauthenticate van szüksége (pl. Adjon meg egy jelszót) még egyszer.

Számos szervezet számítson arra, hogy szoftver alkalmazásokként egy szolgáltatott szoftverként (SaaS) például Office 365, a mezőben és a Salesforce a végfelhasználó hatékonyságát. Hagyományosan informatikai személyzetet tart fenn kell tooindividually létrehozása és frissítése minden SaaS-alkalmazás felhasználói fiókokat, és a felhasználóknak jelszót kelljen megadniuk tooremember minden Szolgáltatottszoftver-alkalmazáshoz.

Az Azure Active Directory kiterjeszti a helyszíni Active Directory hello felhőbe engedélyezése a felhasználók toouse a szervezeti fiók elsődleges toonot csak bejelentkezési tootheir tartományhoz csatlakoztatott eszközökre és a vállalati erőforrásokhoz, hanem összes hello web- és SaaS-alkalmazásokhoz a feladat szükséges.

Így nem csak felhasználók nincs toomanage több példányban felhasználónevek és jelszavak, az alkalmazások hozzáférési automatikusan kiépítése és rendszer leépíti alapján a szervezet csoport tagjai, valamint azok állapotát, az alkalmazottak. Az Azure Active Directory biztonsági vezet be, és hozzáférési irányítás vezérlők, amelyek lehetővé teszik, hogy toocentrally kezelését a felhasználói hozzáférés SaaS-alkalmazásokhoz.

Az Azure AD lehetővé teszi, hogy az egyszerű integráció toomany mai népszerű Szolgáltatottszoftver-alkalmazáshoz; az identitás- és hozzáférés-felügyeletet biztosít és lehetővé teszi a felhasználók toosingle bejelentkezés tooapplications közvetlenül, vagy felderítése és elindíthatja azokat a portálról, például az Office 365-öt vagy hello Azure AD hozzáférési panel.

a következő négy fő építőelemeket hello hello architektúra hello integráció foglalja magában:

* Egyszeri bejelentkezés lehetővé teszi, hogy felhasználók tooaccess azok alapján saját szervezeti fiókjukba, az Azure AD SaaS-alkalmazásokhoz. Egyszeri bejelentkezés mi lehetővé teszi, hogy a felhasználók tooauthenticate tooan alkalmazás a egyetlen szervezeti fiókjával.
* Lehetővé teszi a felhasználók átadása, felhasználói üzembe helyezést és megszüntetést, a Szolgáltatottszoftver-alapú Windows Server Active Directory és/vagy az Azure AD a végrehajtott módosításokat a cél. A kiépített fiók pedig mi lehetővé teszi, hogy egy felhasználó jogosult toobe toouse kérelmet, egyszeri bejelentkezést keresztül hitelesített után.
* Központi hozzáférési alkalmazáskezelése hello Azure felügyeleti portál lehetővé teszi, hogy egyetlen pont, SaaS-alkalmazás-hozzáférés és a kezelés, az hello képességét toodelegate alkalmazás hozzáférés döntési elvégzése és jóváhagyások tooanyone hello szervezet
* Egységes jelentéskészítés és az Azure ad-ben a felhasználói tevékenység figyelése

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Hogyan működik az egyszeri bejelentkezés az Azure Active Directoryval?
Amikor egy felhasználó "bejelentkezik" tooan alkalmazást, akkor nyissa meg hitelesítési eljárással szükséges tooprove, hogy azok mondja ki, hogy azok ki. Egyszeri bejelentkezést, nélkül általában ehhez a hello alkalmazás tárolt jelszót írna be, és a hello felhasználó szükséges tooknow ezt a jelszót.

Az Azure AD tooapplications három különböző módon toosign támogatja:

* **Összevont egyszeri bejelentkezést** lehetővé teszi az alkalmazások tooredirect tooAzure AD a felhasználói hitelesítés helyett a saját jelszót kér. Ez támogatott alkalmazások támogatási SAML 2.0, a WS-Federation, vagy az OpenID Connect, például a protokollokat, és hello richest mód egyszeri bejelentkezést.
* **Jelszó-alapú egyszeri bejelentkezést** lehetővé teszi, hogy biztonságos jelszó alkalmazástárolót és visszajátszásos a webes bővítmény vagy mobilalkalmazást használ. Ez hello meglévő bejelentkezési folyamat hello alkalmazás által biztosított használja, de lehetővé teszi, hogy egy rendszergazda toomanage hello jelszavakat, és nem követeli meg hello felhasználói tooknow hello.
* **Meglévő egyszeri bejelentkezés** bármely létező egyszeri bejelentkezést, amely rendelkezik hello alkalmazás, de lehetővé teszi, hogy a következő alkalmazások kapcsolódó toobe toohello Office 365, illetve az Azure AD hozzáférési panel portálok, és is lehetővé teszi, hogy lehetővé teszi, hogy az Azure AD tooleverage További jelentéskészítés az Azure AD, ha hello alkalmazások működését van.

Miután egy felhasználó hitelesített olyan alkalmazás, is szükségük van üzembe helyezve egy fiók rekordot, amely közli hello alkalmazás hello alkalmazás toohave ahol nincs engedélyekkel és hozzáférési szintje hello alkalmazás belül vannak. a fiók rekord kiépítése hello vagy automatikusan is megtörténhet, vagy akkor fordulhat elő manuálisan egy rendszergazda előtt hello felhasználó egyszeri bejelentkezéses hozzáférést biztosítja.

 Egyszeri bejelentkezés módokban és a kiépítés alább olvashat.

### <a name="federated-single-sign-on"></a>Összevont egyszeri bejelentkezést.
Összevont egyszeri bejelentkezés lehetővé teszi a hello felhasználói bejelentkezés lehetővé teszi, hogy a szervezet toobe automatikusan megtörténik a tooa külső SaaS-alkalmazás az Azure ad hello felhasználói fiók adatait az Azure AD használatával.

Ebben a forgatókönyvben már kijelentkezett az Azure AD-be, és azt szeretné, hogy a külső SaaS-alkalmazás által vezérelt tooaccess erőforrások összevonási szükségtelenné hello egy felhasználó toobe újra hitelesíteni.

Az Azure AD is támogatja az összevont egyszeri bejelentkezést, amely támogatja a WS-Federation, SAML 2.0 hello alkalmazásokkal vagy OpenID connect protokollokat.

További információ: [tartozó tanúsítványok összevont egyszeri bejelentkezést.](active-directory-sso-certs.md)

### <a name="password-based-single-sign-on"></a>Jelszó-alapú egyszeri bejelentkezést.
Jelszó-alapú egyszeri bejelentkezés konfigurálása lehetővé teszi a hello felhasználók a szervezet toobe automatikusan megtörténik a tooa külső SaaS-alkalmazás által az Azure AD-felhasználói fiókadatok hello hello külső Szolgáltatottszoftver-alkalmazás használatával. Ha engedélyezi ezt a szolgáltatást, az Azure AD gyűjt, és biztonságos helyen tárolja a hello felhasználói fiókkal kapcsolatos információk és hello kapcsolódó jelszót.

Az Azure AD támogatására képes, egyszeri bejelentkezési jelszóalapú bármely felhőalapú alkalmazás, amely rendelkezik egy HTML-alapú bejelentkezési oldalára. Egy egyéni böngésző beépülő modul használatával AAD automatizálja a hello felhasználói bejelentkezési folyamathoz keresztül biztonságosan beolvasása az alkalmazás hitelesítő adatait, például a hello felhasználónév és jelszó hello hello könyvtárból, és ezeket a hitelesítő adatokat köt hello alkalmazás bejelentkezési oldal hello felhasználó nevében. Nincsenek két használati esetek:

1. **Kezeli a rendszergazdai hitelesítő adatokat** – rendszergazdák hozhat létre és kezelheti az alkalmazás hitelesítő adatait, és ezen hitelesítő adatok toousers vagy csoportokat, amelyekre szüksége toohello alkalmazás elérése. Ezekben az esetekben hello végfelhasználói kell tooknow hello hitelesítő adatokat, de továbbra is átveszi az egyszeri bejelentkezéses hozzáférést toohello alkalmazás a hozzáférési panel között, illetve a megadott hivatkozásra kattintva. Ez lehetővé teszi mindkét életciklus-felügyeletének hello hitelesítő hello rendszergazdák, valamint kényelmi által azon végfelhasználók számára, amelyek nem kell tooremember és alkalmazás-specifikus jelszavak kezelése. hello hitelesítő adatokat a hello végfelhasználói vannak rejtjelezett során hello automatikus bejelentkezési folyamathoz; azonban technikailag felderíthető hello felhasználó webes hibakeresés eszközök, és a felhasználók és rendszergazdák érdemes követnie ugyanazt biztonsági házirendek, mintha hello hitelesítő adatok hello infrastruktúrakialakítás közvetlenül hello felhasználó által. Rendszergazda által megadott hitelesítő adatok nagyon hasznosak, ha sok felhasználó, például a közösségi média vagy dokumentumok megosztása az alkalmazások által közösen használt fiók számára hozzáférést biztosít.
2. **Kezeli a felhasználói hitelesítő adatokat** – rendszergazdák is alkalmazások tooend felhasználó vagy csoport hozzárendelése, és lehetővé teszi a hello befejező felhasználóknak tooenter hello alkalmazás hello első alkalommal járnak a hozzáférési panel elérése után a saját hitelesítő adatait. Végfelhasználók számára, amellyel nincs szükségük toocontinually meg hello alkalmazásspecifikus jelszavak hello alkalmazás minden alkalommal létrejön a könnyebb elérhetőség érdekében. A használati eset almodell kő tooadministrative felügyeleti hello hitelesítő adatokat, amelyek hello rendszergazdák állíthatják hello alkalmazáshoz új hitelesítő adatokat egy későbbi időpontban hello app hozzáférés hello végfelhasználói élménye módosítása nélkül is használható.

Mindkét esetben a hitelesítő adatok titkosítására hello könyvtárban vannak tárolva, és csak átadott HTTPS-KAPCSOLATON keresztül hello automatikus bejelentkezési folyamat során. Jelszó-alapú egyszeri bejelentkezéshez használ, az Azure AD egy kényelmes identitáskezelési hozzáférési megoldás nem képes a összevonási protokollok alkalmazások kínál.

Jelszó-alapú egyszeri Bejelentkezést egy böngésző bővítmény toosecurely lekérése hello alkalmazás- és felhasználói származó azon információkat az Azure AD alapul, és alkalmazza azt toohello szolgáltatás. A legtöbb külső SaaS-alkalmazásokhoz az Azure AD által támogatott támogatja ezt a szolgáltatást.

Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:

* Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb (lásd még: [IE bővítmény telepítési útmutató](active-directory-saas-ie-group-policy.md))
* Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb
* Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió

**Megjegyzés:** hello jelszóalapú SSO bővítmény is elérhető lesz a Windows 10-es szegély Ha böngészőbővítményeket szegély lesz támogatott.

### <a name="existing-single-sign-on"></a>Meglévő egyszeri bejelentkezést.
Egyszeri bejelentkezés egy alkalmazás konfigurálásakor hello Azure felügyeleti portált biztosít egy harmadik beállítás, a "meglévő egyszeri bejelentkezéshez". Ez a beállítás egyszerűen lehetővé teszi, hogy a rendszergazda toocreate hello egy kapcsolat tooan alkalmazás, és helyezze el a kijelölt felhasználók hello hozzáférési panel.

Például ha egy alkalmazást, amely konfigurált tooauthenticate felhasználók Active Directory összevonási szolgáltatások 2.0 eszköz használatával, a rendszergazda használhat hello "meglévő egyszeri bejelentkezéshez" beállítás toocreate egy hivatkozás tooit hello hozzáférési panel. Amikor a felhasználók hozzáférnek a hello hivatkozás, hitelesített Active Directory összevonási szolgáltatások 2.0 eszköz, vagy bármilyen meglévő egyszeri bejelentkezés megoldás hello alkalmazás biztosítja.

### <a name="user-provisioning"></a>A felhasználók átadása
Jelölje be az alkalmazásokat az Azure AD lehetővé teszi, hogy felhasználói automatizált üzembe helyezést és megszüntetést, fiókok külső SaaS-alkalmazásokhoz az belül hello Azure felügyeleti portálon, a Windows Server Active Directory vagy az Azure AD identity információk alapján. Amikor a felhasználó engedélyeket kap az Azure AD egy ezeket az alkalmazásokat, egy fiók lehet automatikusan létrehozni (kiosztott) hello cél SaaS-alkalmazáshoz.

A felhasználót törlik, vagy adataikat módosítja az Azure ad-ben, ezeket a módosításokat is megjelennek hello SaaS-alkalmazáshoz. Ez azt jelenti, automatikus identitás életciklus-felügyeletének beállítása lehetővé teszi a rendszergazdák toocontrol és adja meg az automatizált üzembe helyezést és megszüntetést az SaaS-alkalmazásokhoz. Az Azure AD-e identitás életciklus-felügyeletének automatizálását szerint engedélyezve van a felhasználók átadása.

több, lásd: toolearn [automatizált Felhasználókiépítése és -megszüntetés tooSaaS alkalmazások](active-directory-saas-app-provisioning.md)

## <a name="get-started-with-hello-azure-ad-application-gallery"></a>Ismerkedés a hello Azure AD application gallery
Készen áll a tooget el? toodeploy egyszeri bejelentkezés az Azure AD között és a szervezet által használt, SaaS-alkalmazásokhoz az alábbiakra.

### <a name="using-hello-azure-ad-application-gallery"></a>Hello Azure AD application gallery használatával
Hello [Azure Active Directory Alkalmazáskatalógusában](https://azure.microsoft.com/marketplace/active-directory/all/) felsorolja az alkalmazások, amelyről ismert, toosupport egy formája, amelyet az egyszeri bejelentkezés az Azure Active Directoryban.

![][1]

Az alábbiakban néhány tipp által milyen lehetőségek támogatják-e alkalmazások kereséséhez:

* Az Azure AD támogatja az automatikus üzembe helyezést és megszüntetést az összes "Kiemelt" alkalmazáshoz a hello [Azure Active Directory Alkalmazáskatalógusában](https://azure.microsoft.com/marketplace/active-directory/all/).
* Összevont kifejezetten támogató alkalmazások listájának összevont egyszeri bejelentkezést SAML, WS-Federation, például protokoll használatát, vagy az OpenID Connect található [Itt](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Ha az alkalmazás már megtalálta, végrehajthatja az első lépéseket hajtsa végre hello részletes utasításokkal hello alkalmazásgyűjtemény állnak rendelkezésre, és hello Azure felügyeleti portál tooenable az egyszeri bejelentkezés.

### <a name="application-not-in-hello-gallery"></a>Alkalmazás nem galéria hello?
Ha az alkalmazás nem található hello Azure AD application gallery, majd telepítette ezeket a beállításokat:

* **Használja fel nem sorolt alkalmazás hozzáadása** -használata hello egyéni kategóriájában hello alkalmazásgyűjtemény belül hello Azure felügyeleti portál tooconnect egy fel nem sorolt alkalmazás, amely a szervezet használja. Bármely alkalmazás, amely támogatja az SAML 2.0, mint egy összevont alkalmazás vagy a bármely alkalmazás, amely rendelkezik egy HTML-alapú bejelentkezési oldal jelszó SSO alkalmazásként is hozzáadhat. További részletekért lásd: Ez a cikk a [saját alkalmazás hozzáadása](active-directory-saas-custom-apps.md).
* **Adja hozzá a saját alkalmazást fejleszt** – Ha hello alkalmazás kialakítása, hajtsa végre a hello irányelveinek hello Azure AD fejlesztői dokumentáció tooimplement összevont egyszeri bejelentkezést, vagy az Azure AD graph API-t használó telepítés hello. További információkért lásd: ezek az erőforrások:
  
  * [Hitelesítési forgatókönyvek az Azure AD-hez](active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Az alkalmazásintegráció kérelem** -kérelmek használatával kell hello alkalmazás támogatása hello [az Azure AD-visszajelzési fórumon](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-hello-azure-management-portal"></a>Hello Azure felügyeleti portál használatával
Hello Active Directory-bővítményt hello Azure felügyeleti portálon tooconfigure hello alkalmazás egyszeri bejelentkezéshez használható. Első lépésként kell tooselect egy könyvtárat, az Active Directory szakasz hello portálon hello:

![][2]

toomanage a külső SaaS-alkalmazásokhoz, megváltoztathatja a hello alkalmazások lap hello kijelölt könyvtár. Ez a nézet lehetővé teszi a rendszergazdáknak:

* Hello Azure AD gyűjteménye, valamint fejleszt alkalmazásokat ki új alkalmazások hozzáadása
* Integrált alkalmazások törlése
* Már van integrálva hello alkalmazások kezelése

Egy külső SaaS-alkalmazáshoz a szokásos felügyeleti feladatok a következők:

* Egyszeri bejelentkezés az Azure AD, Egyszeri jelszóval, vagy SaaS hello cél érhető el, ha összevont egyszeri bejelentkezés engedélyezése
* Szükség esetén a felhasználók átadásához felhasználó üzembe helyezést és megszüntetést (identity életciklus-felügyeletének) engedélyezése
* Alkalmazások esetében, ahol a felhasználók átadása engedélyezve van-e, mely felhasználók férhessenek kiválasztásával toothat alkalmazás

Gyűjtemény-alkalmazásokhoz, amely támogatja az összevont egyszeri bejelentkezést konfigurációs általában meg tooprovide további konfigurációs beállítások, például a tanúsítványok és a metaadatok toocreate összevont megbízhatósági kapcsolat hello külső alkalmazások és az Azure AD között. hello konfiguráló varázsló végigvezeti hello részleteit, és való könnyű hozzáférés toohello SaaS-alkalmazás-specifikus adatokat, és útmutatást nyújt.

Gyűjteményelem alkalmazások, amelyek támogatják az automatikus a felhasználók átadása ehhez meg toogive az Azure AD engedélyeket toomanage fiókjának hello SaaS-alkalmazáshoz. Legalább az Azure AD hitelesítő adatait használja, ha toohello célalkalmazás keresztüli hitelesítést tooprovide szüksége van. E további konfigurációs beállítások megadott toobe függenek hello hello alkalmazás követelményeinek.

## <a name="deploying-azure-ad-integrated-applications-toousers"></a>Üzembe helyezése az Azure AD integrált alkalmazások toousers
Az Azure AD többféleképpen testre szabható toodeploy alkalmazások tooend-a szervezetében lévő felhasználók:

* Az Azure AD hozzáférési panel
* Az Office 365 alkalmazásindító
* Közvetlen bejelentkezés toofederated alkalmazások
* Mélyhivatkozással toofederated, jelszóalapú, vagy meglévő alkalmazásokhoz

Közül melyik metódussal választja toodeploy a szervezet.

### <a name="azure-ad-access-panel"></a>Az Azure AD hozzáférési panel
Hozzáférési Panel hello https://myapps.microsoft.com, egy webes portál, amely lehetővé teszi, hogy a felhasználó szervezeti fiókkal az Azure Active Directory tooview, és indítsa el a felhőalapú alkalmazások toowhich lett volna hozzáféréssel hello Azure AD által rendszergazda. Ha a felhasználó [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), az önkiszolgáló csoportkezelési képességeinek hello hozzáférési Panel keresztül is használhatja.

![][3]

Hozzáférési Panel hello elkülönül hello Azure felügyeleti portálon, és nem igényel felhasználók toohave egy Azure-előfizetés vagy az Office 365-előfizetéssel.

Hello Azure AD hozzáférési panel további információkért lásd: hello [bemutatása toohello hozzáférési panel](active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Az Office 365 alkalmazásindító
A szervezet számára, amely az Office 365 telepített alkalmazások az Azure AD keresztül toousers kijelölt is megjelenik, https://portal.office.com/myapps hello Office 365 portálon. Így könnyen és a felhasználók tetszés szerinti egy szervezet toolaunch az alkalmazások anélkül, hogy toouse egy második portált, és ajánlott hello alkalmazás indítása a szervezet számára az Office 365-tel megoldás.

![][4]

Hello Office 365 alkalmazásindító kapcsolatos további információkért lásd: [rendelkezik Office 365 hello alkalmazás indító jelennek meg az alkalmazás](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-toofederated-apps"></a>Közvetlen bejelentkezés toofederated alkalmazások
Legtöbb összevont alkalmazásokhoz, amelyek támogatják a SAML 2.0, a WS-Federation vagy az OpenID connect is támogatási hello lehetőségét, hogy a felhasználók toostart: hello alkalmazás, és majd beolvasása bejelentkezett keresztül az Azure AD automatikus átirányítása vagy egy hivatkozás toosign a kattintva. Ez az úgynevezett szolgáltató-bejelentkezés kezdeményezett, és a legtöbb összevont alkalmazások hello Azure AD application gallery támogatja ezt a (a dokumentációban hello kapcsolódó hello app egyszeri bejelentkezés konfigurációs varázslójával hello Azure felügyeleti portálon adatokat).

![][5]

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Közvetlen bejelentkezés hivatkozások összevont, jelszóalapú vagy meglévő alkalmazásokhoz
Az Azure AD is támogatja a közvetlen egyszeri bejelentkezés hivatkozások tooindividual alkalmazásokat, amely támogatja a jelszó-alapú egyszeri bejelentkezést, a meglévő egyszeri bejelentkezést és a bármely formájára vonatkozó összevont egyszeri bejelentkezést.

Ezek a hivatkozások akkor kifejezetten kialakított URL-címek, amelyek a felhasználó keresztül hello Azure AD bejelentkezési folyamathoz egy adott alkalmazáshoz, anélkül, hogy a felhasználó hello hello Azure AD hozzáférési panel vagy Office 365 elindítani őket. Egyszeri bejelentkezési URL-található bármely hello hello Azure felügyeleti portált, az Active Directory szakaszában előre integrált alkalmazás hello irányítópult lapján az alábbi hello képernyőfelvételen látható módon.

![][6]

Ezek a hivatkozások másolhatók, és beillesztett bárhol azt szeretné, hogy a bejelentkezés tooprovide kapcsolat toohello kiválasztott alkalmazás. Ennek oka az lehet, e-mailben, vagy a web-alapú bármilyen egyéni portálon, hogy a felhasználó az alkalmazások elérésére vonatkozó beállítása. Íme egy példa egy Azure AD közvetlen egyetlen bejelentkezési URL-CÍMÉT a Twitteren:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Hasonló tooorganization-specifikus URL-címéből hello hozzáférési panelre, még jobban testre szabható az URL-cím a címtáron aktív vagy ellenőrzött tartományok hello egyik után hello myapps.microsoft.com tartomány hozzáadásával. Ez biztosítja a vállalati védjegyek be van töltve azonnal hello bejelentkezési oldal kellene tooenter a felhasználói azonosító először hello felhasználó nélkül:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Amikor egy alkalmazás-specifikus csatolást hitelesített felhasználó kattint, akkor először tekintse meg a szervezeti bejelentkezési oldalára (feltéve, hogy nem már bejelentkezés), és bejelentkezés után átirányított tootheir app hello hozzáférési panelre, először leállítása nélkül. Ha hello felhasználói hiányoznak az Előfeltételek tooaccess hello alkalmazás, például a jelszó-alapú egyszeri bejelentkezési bővítmény hello, hello hivatkozás hello felhasználói tooinstall hello hiányzó bővítmény fogja kérni. hello hivatkozás URL-CÍMÉT is megmarad, ha hello egyszeri bejelentkezés alkalmazásához tartozó konfiguráció hello megváltozik.

Ezek a hivatkozások használata hello azonos hozzáférés mechanizmusai hello, lépjen a panel és az Office 365, és csak ezeket a felhasználókat vagy csoportokat, amelyekre toohello alkalmazás hello Azure felügyeleti portálján van rendelve tudnak toosuccessfully hitelesítéséhez. Azonban bármely felhasználó, aki nem jogosult üzenet jelenik meg azzal az információval, hogy ezek nem rendelkezik hozzáféréssel, és egy hivatkozás tooload hello panel tooview elérhető alkalmazásokat, amelynek érik vannak megadva.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [A Cloud App Discovery megállapítás nem engedélyezett a felhőalapú alkalmazásokhoz](active-directory-cloudappdiscovery-whatis.md)
* [Bevezetés tooManaging hozzáférés tooApps](active-directory-managing-access-to-apps.md)
* [Az Azure AD külső identitások kezelésére szolgáló képességeket összehasonlítása](active-directory-b2b-compare-external-identities.md)

<!--Image references-->
[1]: ./media/active-directory-appssoaccess-whatis/onlineappgallery.png
[2]: ./media/active-directory-appssoaccess-whatis/azuremgmtportal.png
[3]: ./media/active-directory-appssoaccess-whatis/accesspanel.png
[4]: ./media/active-directory-appssoaccess-whatis/officeapphub.png
[5]: ./media/active-directory-appssoaccess-whatis/workdaymobile.png
[6]: ./media/active-directory-appssoaccess-whatis/deeplink.png
