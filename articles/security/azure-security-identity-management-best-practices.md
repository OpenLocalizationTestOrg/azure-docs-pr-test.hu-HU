---
title: "aaaAzure identity & access ajánlott biztonsági eljárások |} Microsoft Docs"
description: "Ez a cikk számos gyakorlati tanácsok az Identitáskezelés, és a beépített hozzáférés-vezérlése segítségével Azure-képességek."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: yurid
ms.openlocfilehash: af07dfda84758b9124641078ac8f696f725f2bf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Az Azure Identitáskezelés és hozzáférés szabályozása ajánlott biztonsági eljárások
Számos fontolja meg a toobe hello új határ identitásrétegének a biztonság érdekében, hogy a szerepkör hello hagyományos hálózati-központú szempontjából tovább tart. Az elsődleges pivot hello biztonsági figyelmet és beruházások alakulása származhat hello tényt, hogy hálózati kialakítását váltak egyre elválasztó és, hogy külső védelem hatásos voltak egyszer előzetestoohellofelbontásáranemlehet[BYOD](http://aka.ms/byodcg) eszközök és a felhőalapú alkalmazásokhoz.

Ez a cikk ismertetik Azure Identitáskezelés és access control ajánlott biztonsági eljárások gyűjteménye. Az alábbi gyakorlati tanácsok a tapasztalatunk származó [az Azure AD](../active-directory/active-directory-whatis.md) és hello véleményeket ügyfelek, például saját maga.

Az egyes ajánlott eljárás azt ismertetjük:

* Milyen hello ajánlott eljárás
* Miért érdemes tooenable, hogy a legjobb
* Mi lehet hello eredmény, ha Ön nem tooenable hello ajánlott
* Lehetséges alternatívák toohello ajánlott
* Hogyan szerezhet tooenable hello ajánlott

Az Azure-identitás kezelése és hozzáférés szabályozása gyakorlati tanácsok cikk egy együttműködési véleményt és az Azure platform olyan képességeit és a szolgáltatáskészletek, alapuló biztonsági hello során ez a cikk írásának léteznek. Vélemények és technológiák változnak az idők, és ez a cikk lesznek frissítve egy rendszeresen tooreflect ezeket a módosításokat.

Azure-identitás- kezelésben vezérlő ajánlott biztonsági eljárások cikkben említett a következők:

* Az Identitáskezelés központosítása
* Egyszeri bejelentkezés (SSO) engedélyezése
* A jelszókezelés telepítése
* A felhasználók a többtényezős hitelesítést (MFA) kényszerítése
* Használjon szerepköralapú hozzáférés-vezérlést (RBAC)
* Szabályozhatja a helyek, ahol erőforrások jönnek létre erőforrás-kezelő használatával
* Útmutató a fejlesztők tooleverage identitási képességeibe SaaS-alkalmazásokhoz
* Aktívan figyeli, hogy gyanús tevékenységek

## <a name="centralize-your-identity-management"></a>Az Identitáskezelés központosítása
Egy fontos lépés felé biztonságossá tétele az identitás tooensure, hogy az informatikai kapcsolatban, ahol ez a fiók létrejött egy egyetlen helyről felügyelheti fiókjait. Amíg hello vállalkozások informatikai szervezetek többsége hello lesz az elsődleges fiók címtár a helyszínen, hibrid felhőben történő alkalmazáshoz hello okot, és fontos, hogy tudomásul veszi hogyan toointegrate a helyszíni és felhőbeli címtárakban és adjon meg egy zökkenőmentes élményt toohello végfelhasználói.

tooaccomplish ez [hibrid identitás](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) forgatókönyvben javasoljuk két lehetőség közül választhat:

* A helyszíni címtárral szinkronizálja az Azure AD Connect használatával a felhő címtárral
* A felhő directory használatáról a helyszíni identitás összevonni [Active Directory összevonási szolgáltatások](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS)

A szervezeteknek, amelyek a helyszíni identitás a felhőbeli identitással fog tapasztalni toointegrate sikertelen nőtt felügyeleti terhelés kezelésére, ami növeli a hibák és a biztonsági résekkel szemben hello valószínűségét.

További információ az Azure AD szinkronizálási, olvassa el az hello cikk [a helyszíni identitások integrálása az Azure Active Directoryval](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Egyszeri bejelentkezés (SSO) engedélyezése
Ha egyszerre több könyvtárak toomanage, ez lesz-e nem csak a felügyeleti probléma informatikai is azon végfelhasználók számára, amelyet több jelszóra kell tooremember. A [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) adja meg a felhasználók azonos toosign a hitelesítő adatok beállítása és hello van szükségük, attól függetlenül történik Ha ehhez az erőforráshoz a helyszínen található, vagy hello felhőben lévő erőforrások eléréséhez használjon hello hello képességét.

Egyszeri bejelentkezés tooenable felhasználók tooaccess használja a [SaaS-alkalmazásokhoz](../active-directory/active-directory-appssoaccess-whatis.md) alapján saját szervezeti fiókjukba, az Azure ad-ben. Ez a tulajdonság vonatkozik nem csak a Microsoft SaaS-alkalmazásokhoz, emellett pedig más alkalmazások, például a [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) és [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Az alkalmazás is lehet, a konfigurált toouse az Azure AD egy [SAML-alapú identitás](../active-directory/fundamentals-identity.md) szolgáltató. Biztonsági ellenőrzés, mint az Azure AD engedélyezi azok toosign hello alkalmazásba kivéve kaptak az Azure AD hozzáférési jogkivonat nem állítanak ki. Előfordulhat, hogy megadta a hozzáférést közvetlenül, vagy egy csoport tagjai legyenek.

> [!NOTE]
> hello döntési toouse SSO befolyásolja, hogy hogyan integrálja a helyszíni címtár és a a felhő címtárának. Ha azt szeretné, hogy egyszeri Bejelentkezést, szüksége lesz toouse összevonási, mert a címtár-szinkronizálás csak biztosít [ugyanazt a bejelentkezési élményt](../active-directory/active-directory-aadconnect.md).
>
>

A szervezeteknek, amelyek nem kényszerítése egyszeri Bejelentkezést a felhasználók és az alkalmazások több kitett tooscenarios, amelyen felhasználó több jelszóra, növelve a közvetlenül hello annak valószínűségét, hogy a felhasználók újból felhasználja a jelszavak, vagy gyenge jelszóval rendelkezik.

További Azure AD egyszeri Bejelentkezéssel kapcsolatos hello cikk olvasásával [AD FS kezelése és testreszabása az Azure AD Connect](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>A jelszókezelés telepítése
Forgatókönyvekben, ahol több bérlő van, vagy túl szeretné tooenable felhasználók[saját jelszó visszaállítása](../active-directory/active-directory-passwords-update-your-own-password.md), fontos, hogy megfelelő biztonsági házirendek tooprevent visszaélés használja. Az Azure-ban hello önkiszolgáló jelszó-visszaállítási funkció használja, és testre szabhatja a hello biztonsági beállítások toomeet az üzleti igényeknek.

Különösen fontos tooobtain visszajelzés ezektől a felhasználóktól, és ismerje meg a felületéről, ezeket a lépéseket próbálkozhat tooperform. Ezek a tapasztalatok alapján, a terv toomitigate lehetséges problémák egy nagyobb csoport hello telepítése során esetlegesen előforduló kidolgozása. Hello használatát is javasoljuk [jelszó alaphelyzetbe állítása regisztrációs Tevékenységjelentés](../active-directory/active-directory-passwords-get-insights.md) toomonitor hello felhasználó regisztrálja.

Tooavoid jelszó szervezetek támogatási hívások módosítja, de engedélyezi a felhasználók tooreset saját jelszavukat amelyek jobban ki vannak téve tooa magasabb hívás kötet toohello ügyfélszolgálatához toopassword problémák miatt. A több bérlő rendelkező szervezeteknek rendkívül fontos, hogy az ilyen típusú funkció végrehajtására és engedélyezése a felhasználók tooperform jelszó-változtatási hello biztonsági házirendben létrehozott biztonsági határon belül.

További információ a jelszó alaphelyzetbe állítását hello cikk elolvasása [jelszókezelés üzembe helyezése és képzési felhasználók toouse azt](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>A felhasználók a többtényezős hitelesítést (MFA) kényszerítése
Olyan szervezeteknek, amelyek toobe iparági szabványoknak megfelelő kell például [PCI DSS 3.2-es verziójú](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), multi-factor authentication egy kell képességgel a felhasználók hitelesítésére. Az iparágban elfogadott megoldásokhoz tesznek, túl tooauthenticate felhasználók többtényezős hitelesítés kényszerítése is hozzájárulhat a szervezetek toomitigate hitelesítő adatokkal való visszaéléseket típusú támadások, például a [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

A felhasználók számára az Azure MFA engedélyezésével ad hozzá egy második réteg biztonsági toouser bejelentkezéseket és tranzakciókat. Ebben az esetben egy tranzakció lehetséges, hogy használja a dokumentum egy fájlkiszolgálón, vagy a SharePoint Online-ban található. Az Azure MFA informatikai tooreduce hello valószínűsége, hogy a feltört hitelesítő adatot lesz-e a hozzáférés tooorganization adatok is segíti.

Például: Azure többtényezős hitelesítés kényszerítéséhez a felhasználók számára, és konfigurálni toouse egy telefonhívással vagy szöveges üzenettel ellenőrzése. Ha hello felhasználói hitelesítő adataikat megszerzik, hello támadó nem lehet a bármilyen olyan erőforrás képes tooaccess mivel ő nem kell hozzáférést toouser phone. Ne adjon hozzá további identitás védelmi réteget használó szervezetek jobban ki vannak téve a hitelesítő adatok jelszóellopásos támadáshoz, ami azt eredményezheti, toodata sérült biztonság esetén.

Olyan szervezeteknek, amelyek tookeep hello egész hitelesítési vezérlőt egy alternatív helyszíni toouse [Azure multi-factor Authentication kiszolgáló](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), más néven az MFA a helyszínen. Ez a módszer használatával továbbra is meg fogja tudni tooenforce többtényezős hitelesítést, ugyanakkor változatlanul megőrizze a hello MFA kiszolgáló a helyi.

További információ az Azure MFA, olvassa el az hello cikk [Ismerkedés az Azure multi-factor Authentication hello felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Használjon szerepköralapú hozzáférés-vezérlést (RBAC)
Hello alapján történő hozzáférés [tooknow kell](https://en.wikipedia.org/wiki/Need_to_know) és [legalacsonyabb jogosultsági szint](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági elveket elengedhetetlen a szervezeteknek, amelyek szeretné, hogy az adatelérési tooenforce biztonsági házirendeket. Azure szerepköralapú hozzáférés-vezérlés (RBAC) lehet használt tooassign engedélyek toousers, csoportok és alkalmazások egy adott hatókörben. szerepkör-hozzárendelés hello hatóköre lehet előfizetés, egy erőforráscsoport vagy egy erőforrást.

Kihasználhatja [beépített RBAC](../active-directory/role-based-access-built-in-roles.md) szerepkörök az Azure tooassign jogosultságokkal toousers. Érdemes lehet *tárolási fiók közreműködői* a felhő üzemeltetői toomanage tárfiókok igénylő és *klasszikus tárolási fiók közreműködői* szerepkör toomanage klasszikus tárfiókokat. A felhő üzemeltetői, amelyet a toomanage virtuális gépek és a tárfiókot, fontolja meg, azokat túl*virtuális gép közreműködő* szerepkör.

A szervezeteknek, amelyek kényszeríti ki a hozzáférés-vezérlés képességeinek például RBAC által előfordulhat, hogy kell jogosultságot ad mint szükséges tootheir felhasználók több jogosultsággal. Ennek eredményeképpen előfordulhat toodata biztonsági sérülés által felhasználók hozzáférjenek toocertain típusú adattípusok (pl., nagy üzleti jelentőség), amelyek nem rendelkeznek hello első helyen.

További tudnivalók az Azure RBAC hello cikk olvasásával [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Szabályozhatja a helyek, ahol erőforrások jönnek létre erőforrás-kezelő használatával
Engedélyezés felhő operátorok tooperform feladatok közben megakadályozza az egyezmények, amelyek megtörje toomanage szükséges a szervezet erőforrásaihoz nagyon fontos. A szervezetek toocontrol hello helyeken, ahol erőforrások jönnek létre a kívánt merevlemez kell code ezeket a helyeket.

tooachieve, a szervezetek hello műveleteket, vagy kifejezetten elutasított erőforrásokat leíró definíciókkal rendelkező biztonsági házirendeket hozhat létre. E házirend-definíciók szükséges hello hatókörben, például hello előfizetés, erőforráscsoport vagy egy egyéni erőforrást lehet kijelölni.

> [!NOTE]
> Ez van nem hello ugyanaz, mint a Szerepalapú, ténylegesen kihasználja a Szerepalapú tooauthenticate hello rendelkező felhasználók jogosultság toocreate ezeket az erőforrásokat.
>
>

Használja ki az [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toocreate egyéni házirendek is forgatókönyvek, ahol hello szervezet részlege azt szeretné tooallow műveletek csak amikor hello megfelelő költségközpont társított; ellenkező esetben ezek megtagadja hello kérelem.

A szervezeteknek, amelyek nem elsődlegesek erőforrások létrehozását, előfordulhat, hogy van szükségük további erőforrásokat létrehozása hello szolgáltatás való visszaélés jobban ki vannak téve toousers. Egy fontos lépés toosecure egy több-bérlős forgatókönyv korlátozására hello erőforrás létrehozása folyamatban.

Hello cikk olvasásával az Azure Resource Manager házirendek létrehozásával kapcsolatos részletesebb [vezérlési hozzáférési és használati feltételei toomanage erőforrások](../azure-resource-manager/resource-manager-policy.md).

## <a name="guide-developers-tooleverage-identity-capabilities-for-saas-apps"></a>Útmutató a fejlesztők tooleverage identitási képességeibe SaaS-alkalmazásokhoz
Javítható felhasználói identitás számos forgatókönyv felhasználók általi elérésekor [SaaS-alkalmazások](https://azure.microsoft.com/marketplace/active-directory/all/) , amely integrálható a helyszíni vagy felhőalapú könyvtár. Mindenekelőtt, javasoljuk, hogy a fejlesztők egy biztonságos módszert toodevelop ezeket az alkalmazásokat, például [Microsoft biztonságos fejlesztési Életciklussal (SDL)](https://www.microsoft.com/sdl/default.aspx). Az Azure AD hitelesítési egyszerűbbé teszi a fejlesztők azáltal identitás szolgáltatásként, például támogatja az ipari szabványnak számító protokollokat [OAuth 2.0](http://oauth.net/2/) és [OpenID Connect](http://openid.net/connect/), valamint nyílt forráskódú a szalagtárak különböző platformokon.

Győződjön meg arról, hogy tooregister hitelesítési tooAzure AD outsources bármely olyan alkalmazás, ez egy kötelező eljárást. hello OK mögött ennek az oka, hogy az Azure AD toocoordinate hello kommunikációs hello alkalmazással van szüksége, amikor bejelentkezés (SSO) kezelése vagy cseréjét jogkivonatok. hello felhasználói munkamenet lejár, ha az Azure AD által kibocsátott hello jogkivonat élettartamát hello lejár. Mindig értékelje ki, ha az alkalmazás most kell használni, vagy ha a megadott idő csökkentése érdekében. Csökkentési hello élettartamát, amely arra kényszeríti ki a felhasználók toosign adott ideig tartó tétlenség alapuló biztonsági intézkedésként működhet.

A szervezeteknek, amelyek nem érvényesítik identitást vezérlő tooaccess használó alkalmazások nem a és útmutató a fejlesztő hogyan toosecurely integrálása alkalmazások az identity management rendszer lehet, hogy jobban ki vannak téve toocredential lopás típusú támadások, például a [gyenge hitelesítés és a munkamenet felügyeleti megnyitása webes alkalmazás biztonsági Project (OWASP) első 10 ismertetett](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

További hitelesítési forgatókönyvek az SaaS-alkalmazások kapcsolatos olvasásával [hitelesítési forgatókönyvek az Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Aktívan figyeli, hogy gyanús tevékenységek
Túl szerint[Verizon 2016 adatok biztonsági szabályok megsértésére jelentés](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), támadását még mindig van hello megnövekedhet és váljon egyik hello legnyereségesebb vállalkozások számára számítógépes is. Ezért fontos fontos toohave egy aktív figyelő identitásrendszere, amely gyorsan viselkedés gyanús tevékenységek észlelése és további vizsgálatra riasztást vált ki. Azure AD is rendelkezik két főbb képességeket, amelyek segítségével a szervezetek figyelhetik az identitások: prémium szintű Azure AD [anomáliabiztonsági jelentéseket](../active-directory/active-directory-view-access-usage-reports.md) és az Azure AD [identity protection](../active-directory/active-directory-identityprotection.md) funkció.

Győződjön meg arról, hogy toouse hello anomáliabiztonsági jelentéseket tooidentify kísérletek toosign a [nélkül nyomon követve](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [találgatásos](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) támadások elleni egy különös figyelmet kísérletek toosign a több helyről, jelentkezzen be a [fertőzött eszközök](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) és gyanús IP-címeket. Ne feledje, hogy ezek a jelentések. Ez azt jelenti folyamatok és eljárások alapján helyezze el az informatikai rendszergazdák toorun ezekre a jelentésekre hello napi szinten, vagy az igény szerinti (általában egy lévő incidensválasz) kell rendelkeznie.

Ezzel szemben az Azure AD identity protection egy aktív felügyeleti rendszer, és akkor lesz jelzőt hello aktuális kockázatok a saját irányítópultra. Amellett, hogy a is napi összefoglaló értesítést küldjenek e-mailt fog kapni. Azt javasoljuk, hogy hello kockázati szint tooyour üzleti követelményeinek megfelelően beállítani. hello kockázati szintjét a kockázati események hello kockázat esemény súlyossága hello megjelölése (magas, közepes vagy alacsony). hello kockázati szint segít az Identity Protection hello műveleteinek azok kell tooreduce hello kockázati tootheir szervezet rangsorolására.

A szervezeteknek, amelyek aktívan figyeli a identitáskezelési rendszereket, hogy sérült felhasználói hitelesítő adatok veszélyben van. A gyanús tevékenységek tudomása nélkül helyezze el a következő hitelesítő adatokkal, a szervezetek nem tud toomitigate fogja az ilyen típusú fenyegetést.
További tudnivalók az Azure Identity protection olvasásával [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md).
