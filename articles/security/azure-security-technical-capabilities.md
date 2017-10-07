---
title: "aaaAzure biztonsági műszaki képességei |} Microsoft Docs"
description: "További információk a felhőalapú számítási szolgáltatás, amely tartalmazza a számítási példányokért széles kijelölt & szolgáltatások automatikusan toomeet hello igényeinek az alkalmazás vagy a vállalati fel és le is méretezhető."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/26/2017
ms.author: TomSh
ms.openlocfilehash: a0ef17883be54dab4cb6b597204f3197dc05c28c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-technical-capabilities"></a>Az Azure biztonsági technikai lehetőségeit

tooassist jelenlegi és jövőbeli Azure ügyfelek megértéséhez, és felhasználja az hello különböző biztonsági képességek állnak rendelkezésre, és a környező hello Azure platformra, Microsoft kifejlesztette egy sorozatát tanulmányok, biztonsági áttekintése, ajánlott eljárások és Ellenőrzőlisták. hello témakörök tartomány körének és mélysége, és rendszeresen frissülnek. Ez a dokumentum a sorozat része az alábbi hello absztrakt szakaszban foglaltak szerint. További információk az Azure biztonsági adatsorozat (URL) helyen találhatók meg.

## <a name="azure-platform"></a>Azure-platform

[A Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) a felhőplatform magában foglalja az infrastruktúra és a nyilvános felhő adatokat a Microsoft adatközpontjaiban gazdája alkalmazásszolgáltatások, integrált adatszolgáltatások és a speciális elemzés, és a fejlesztői eszközök és a szolgáltatások,. Az ügyfelek használata az Azure számos különböző kapacitások és forgatókönyvekhez készült, az alapvető számítási, hálózati és tárolási, toomobile és a webes alkalmazás szolgáltatásokból, toofull felhő helyzetekben, például az eszközök internetes hálózatát, és használhatók nyílt forráskódú technológiákkal, és központilag telepített hibrid a felhő, vagy az ügyfél adatközponton belül üzemeltetett. Azure nyújt felhőtechnológia építőelemeket toohelp vállalatok költségek csökkentése, gyorsan innovációját és rendszerek proaktív kezelését. Létrehozása vagy áttelepítése informatikai eszközök tooa felhőszolgáltatóként, akkor vannak hagyatkoznia adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatásaival és hello vezérlők toomanage hello biztonsági a felhő alapú eszközök biztosítanak.

Microsoft Azure hello csak felhőalapú számítási szolgáltatót, amelyet egy biztonságos, konzisztens alkalmazás platform és infrastruktúra,--szolgáltatás a csapatok toowork belül a különböző felhőalapú skillsets és a projekt összetettségét, integrált adatok szintjei kínál a szolgáltatások és elemzés, amely fedik le az adatokból az eszközintelligencia bárhol létezik, mind a Microsoft, mind a nem Microsoft-platformokat, nyissa meg a keretrendszerek és az eszközök, biztosítva, hogy a választott felhő integrálása a helyszíni, valamint az Azure felhőszolgáltatások belül telepítése a helyszíni adatközpontokkal. Microsoft megbízható felhő hello részeként ügyfelek támaszkodnak Azure iparágvezető biztonsági, megbízhatósági, megfelelőség, adatvédelmi és hello túlnyomó hálózati felhasználók, partnerek és folyamatok toosupport szervezetek hello felhőben.

A Microsoft Azure-ban a következőket teheti:

- A hello felhőalapú innováció érdekében.

- Energiagazdálkodási üzleti döntéseket &, amelyen az alkalmazásokat.

- Szabadon és üzembe helyezheti tetszőleges helyre.

- Az üzleti védelme.

## <a name="scope"></a>Hatókör

e tanulmány hello kapcsolattartási ponton vonatkozik, biztonsági szolgáltatásokat és funkciókat támogatja a Microsoft Azure alapvető összetevői, nevezetesen [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction), [Microsoft Azure SQL-adatbázisok](https://docs.microsoft.com/azure/sql-database/), [Microsoft Azure virtuális gép modell](https://docs.microsoft.com/azure/virtual-machines/  ), és hello eszközök és infrastruktúra, amely minden felügyeletét. Ez a dokumentum helyezi a hangsúlyt a Microsoft Azure technikai képességekkel elérhető tooyou ügyfelek toofulfil betöltött szerepük hello biztonsági és az adatok védelme érdekében.

a megosztott felelősségi modell ismertetése hello fontosságát elengedhetetlen az ügyfelek, akik toohello felhő áthelyezni. Felhőbeli szolgáltatók biztonsági és megfelelőségi erőfeszítéseket jelentős előnyeit, de ezeket az előnyöket nem absolve a felhasználók, az alkalmazások és a szolgáltatásajánlatok védjék hello ügyfél.

IaaS-megoldásokat hello ügyfél felelős, vagy egy megosztott felelős biztonságossá tétele és hello operációs rendszer, hálózati konfigurációt, alkalmazások, identitás, az ügyfelek és adatok kezelése.  PaaS megoldások létrehozásához, IaaS központi telepítések, hello az ügyfél továbbra is felelős, vagy egy megosztott felelős biztonságossá tétele és az alkalmazások, identitás, az ügyfelek és adatok kezelése. A Szolgáltatottszoftver-megoldások, Nonetheless, hello ügyfél továbbra is toobe felelős. Ezek győződjön meg arról, hogy az adatok besorolásának megfelelően, és a felhasználók és eszközök végponti osztoznak egy felelős toomanage.

Ez a dokumentum nem biztosít részletes hello bármelyikének kapcsolódó Microsoft Azure platform összetevők, például Azure-webhelyek, Azure Active Directory, HDInsight, a Media Services, és más szolgáltatásokat elveire hello alapösszetevőket réteges vannak. Bár a legkisebb általános információkat, olvasók ismeri az Azure rendszerhez kapcsolódó alapfogalommal mutató egyéb hivatkozásokat a Microsoft által biztosított, és ez a dokumentum a megadott hivatkozások szerepelnek a rendszer feltételezi.


## <a name="available-security-technical-capabilities-toofulfil-user-customer-responsibility---big-picture"></a>Biztonsági technikai képességekkel toofulfil (ügyfél) felhasználó felelőssége - nagy vonalakban tekinti

A Microsoft Azure biztosít, amelyekkel az ügyfelek felelniük hello biztonsági, adatvédelmi és megfelelőségi igényeinek. hello segítségével ismertetik a különböző Azure számára elérhető szolgáltatásokat felhasználók toobuild biztonságosságának és alkalmazás-infrastruktúra szabványokon alapuló kép követően.

![Biztonsági technikai képességekkel-nagy kép](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Kezelhetik és szabályozhatják az identitás- és felhasználói hozzáférést (védelem)

Azure segítséget nyújt a vállalati és személyes adatok védelme akkor toomanage felhasználói identitások és hitelesítő adatokat és elérés engedélyezése.

### <a name="azure-active-directory"></a>Az Azure active directory

Microsoft identitások és hozzáférések felügyeleti megoldások Súgó informatikai hozzáférés tooapplications és erőforrások védelmében hello vállalati adatközpontban és hello felhőre, például a többtényezős hitelesítés és a feltételes érvényesítési további szinteket engedélyezése hozzáférési házirendek. Gyanús tevékenységek figyelése keresztül speciális biztonsági jelentések, a naplózás és a riasztási segít mérsékelni a potenciális biztonsági problémákat. [Az Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) biztosítja az egyszeri bejelentkezés toothousands felhőalapú (SaaS) alkalmazások és az access tooweb alkalmazások helyi futtatása.

Biztonsági szempontból előnyökkel járhat az Azure Active Directory (AD) megadhatják hello:

- Hozzon létre, és egy egyetlen identitást az egyes felhasználók kezeléséhez a hibrid vállalati, a felhasználók, csoportok és az eszközök szinkronban tartja.

- Egyszeri bejelentkezéses hozzáférést több ezer előre integrált Szolgáltatottszoftver-alkalmazásoknál például tooyour alkalmazások biztosítása.

- Engedélyezze a hozzáférést alkalmazásbiztonsági mind a helyszíni szabályalapú többtényezős hitelesítés kényszerítése, és a felhőalapú alkalmazásokhoz.

- Kiépítés biztonságos távoli hozzáférés tooon helyszíni webes alkalmazások az Azure AD-alkalmazásproxy használatával.

[Az Azure active directory portálon](http://aad.portal.azure.com/) érhető el az azure portál egy részét. Erről az irányítópultról áttekintheti a szervezet hello állapotát, és könnyen alaposabban hello directory, a felhasználók vagy az alkalmazás-hozzáférés kezelése.

![Az Azure active directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Következő vannak alapvető Azure identitáskezelési képességeit:

- Egyszeri bejelentkezés

- Multi-Factor Authentication

- Biztonsági figyelést, a riasztások és a machine learning-alapú jelentések

- A felhasználói identitások és hozzáférés kezelése

- Eszközregisztráció

- Privileged identity management

- Identity Protection

#### <a name="single-sign-on"></a>Egyszeri bejelentkezés

[Egyszeri bejelentkezés (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) azt jelenti, hogy képes tooaccess alatt álló összes hello alkalmazásokat és erőforrásokat, hogy kell-e toodo üzleti történő bejelentkezéssel csak egyszer egyetlen felhasználói fiókkal. Miután bejelentkezett, anélkül, hogy a szükséges tooauthenticate van szüksége valamennyi hello alkalmazások végezheti el (például adjon meg egy jelszót) még egyszer.

Számos szervezet alkalmazásokként egy szolgáltatott szoftverként (SaaS) például Office 365, a mezőben és a Salesforce a végfelhasználói termelékenység szoftver támaszkodnak. Hagyományosan informatikai munkatársak szükséges tooindividually létrehozása és frissítése minden SaaS-alkalmazás felhasználói fiókokat, és felhasználók tooremember minden SaaS-alkalmazáshoz tartozó jelszót.

[Az Azure AD kiterjeszti a helyszíni Active Directory hello felhőbe](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), engedélyezése a felhasználók toouse az elsődleges szervezeti fiók toonot csak bejelentkezési tootheir tartományhoz csatlakoztatott eszközökre és a vállalati erőforrásokhoz, de is összes hello web- és SaaS-alkalmazásokhoz a feladat szükséges.

Nem csak felhasználók nem rendelkeznek toomanage több példányban felhasználónevei és jelszavai, alkalmazás-hozzáférés csak alapján automatikusan kiosztott vagy vonja kiosztott szervezeti csoportok és az állapotuk egy alkalmazott. [Az Azure AD vezet be a biztonsági és irányítási vezérlők](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) , amelyek lehetővé teszik toocentrally hozzáférést kezeli a Szolgáltatottszoftver-alkalmazáshoz.

#### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

[Az Azure többtényezős hitelesítés (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) hello egynél több ellenőrzési módszer használatát igényli, és a kritikus fontosságú második réteget biztonsági toouser bejelentkezéseket és tranzakciókat ad hitelesítési módszer. [Többtényezős hitelesítés segítségével a biztonságos működés érdekében](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) toodata és az alkalmazások eléréséhez egyszerű bejelentkezési folyamatot a felhasználó igény szerint betartása mellett. Erős hitelesítés, ellenőrzési lehetőségek széles keresztül biztosítja – a telefonhívás, szöveges üzenet vagy mobilalkalmazás értesítés vagy ellenőrző kód és a külső OAuth jogkivonatokat.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Biztonsági figyelést, a riasztások és a machine learning-alapú jelentések

A biztonság ellenőrzése és a riasztások és a machine learning-alapú jelentések, amelyek azonosítják az inkonzisztens hozzáférési mintázatokat segítségével védelmet az üzleti. Használhatja az Azure Active Directory hozzáférési és használati jelentések toogain láthatósága hello adatintegritási és biztonsági a szervezete címtárát. Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat, hogy azok megfelelően megtervezheti toomitigate kockázatok vizsgálandó.

A klasszikus Azure portálon hello vagy azon keresztül [Azure Active directory portálon](http://aad.portal.azure.com/), [jelentések](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) szerint vannak kategóriába sorolva hello a következő módon:

- Anomáliadetektálási jelentések – jelentkezzen be, hogy észleltünk toobe rendellenes eseményeket tartalmazza. Célunk toomake tud-e a tevékenység, és lehetővé teszik a toobe képes toodecide arról, hogy az esemény gyanúsnak.

- Integrált alkalmazás jelentések – betekintést, hogyan használja a szervezet a felhőalapú alkalmazásokhoz. Az Azure Active Directory integrálható a felhőalapú alkalmazások ezer.

- Hibajelentések – fiókok tooexternal alkalmazások létesítésekor előforduló hibákat jelzik.

- Felhasználó-specifikus jelentései – eszköz/sign tevékenységek adatai egy adott felhasználó jelenítenek meg.

- Tevékenységi naplóit – tartalmazhat Feljegyzés hello belül minden naplózott események az elmúlt 24 órában, az utolsó 7 napig, vagy utolsó 30 nap során, és a csoport tevékenység módosításainak és jelszó alaphelyzetbe állítása és nyilvántartási tevékenység.

#### <a name="consumer-identity-and-access-management"></a>A felhasználói identitások és hozzáférés kezelése

[Az Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) magas rendelkezésre állású, globális, identitás felügyeleti szolgáltatás a felhasználók felé néző alkalmazások identitások millióinak toohundreds méretezi. Mobil- és webes platformokba is integrálható. A felhasználók bejelentkezhetnek tooall, testre szabható felhasználói élmény mellett az alkalmazások új vagy meglévő közösségi fiókjaik használatával.

A múltbeli alkalmazásfejlesztők számára túl hello[és a felhasználók bejelentkezését](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) alkalmazásokba integrálhassák volna írt saját kódot. És ezeket használja, a helyszíni adatbázisokat vagy rendszereket toostore felhasználóneveket és jelszavakat. Az Azure Active Directory B2C kínál a szervezet egy jobb módon toointegrate felhasználói Identitáskezelés alkalmazásokba hello segítségével biztonságos, szabványokon alapuló platformja és bővíthető szabályzatainak számos.

Azure Active Directory B2C használata esetén a felhasználók regisztrálhatnak az alkalmazások új hitelesítő adatok (e-mail címet és jelszót, vagy felhasználónév és jelszó) létrehozásával vagy meglévő közösségi fiókjaik használatával (Facebook, Google, Amazon, LinkedIn).

Eszközregisztráció

[Az Azure AD Eszközregisztrációval](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) hello alapja eszközalapú [feltételes hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) forgatókönyvek. Amikor regisztrál egy eszközt, az Azure Active Directory Eszközregisztrációs biztosít hello eszköz, amely használt tooauthenticate hello eszköz hello felhasználó bejelentkezésekor identitással. hello hitelesített eszköz és hello eszköz attribútumai – hello, majd lehet használt tooenforce feltételes hozzáférési házirendek hello felhő és a helyszínen tárolt alkalmazások esetében.

Együtt egy [mobileszköz-kezelés (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) megoldással, például az Intune-ban, az Azure Active Directoryban hello eszközattribútumokon frissítődik hello eszközzel kapcsolatos további információk. Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak.

#### <a name="privileged-identity-management"></a>Privileged identity management

[Az Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) lehetővé teszi, hogy a kezelése, szabályozása, és figyelje az emelt szintű identitások és tooresources elérni az Azure ad-ben, valamint egyéb Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.

Néha a felhasználóknak kell toocarry ki az Azure vagy az Office 365 erőforrásokhoz, vagy más Szolgáltatottszoftver-alkalmazásoknál privilegizált műveleteket. Ez gyakran azt jelenti, a szervezetek toogive őket állandó jogosultsági szintű hozzáférés az Azure ad-ben. Ez a felhőben üzemeltetett erőforrásokhoz az egyre növekvő biztonsági kockázatot jelent, mert a szervezeteknek elég nem tud figyelni, ezek a felhasználók tevékenységeit a rendszergazda jogosultságokkal. Továbbá ha jogosultsági szintű hozzáféréssel rendelkező felhasználói fiók biztonsága sérül, egy megsértésének jelentős hatással lehet a felhő átfogó biztonsági. Az Azure AD Privileged Identity Management segít tooresolve a kockázat.

Az Azure AD Privileged Identity Management lehetővé teszi:

- Mely felhasználók vannak-e az Azure AD-rendszergazdák

- Engedélyezze az igény, "csak az időben" rendszergazdai hozzáférés tooMicrosoft Online szolgáltatások, például Office 365 és az Intune-ban

- Rendszergazda-hozzárendelések beolvasása a rendszergazdai hozzáférési műveleteiről és a változások

- Értesítéskérés a hozzáférés tooa kiemelt szerepkörű

#### <a name="identity-protection"></a>Identity Protection

[Az Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) biztonsági szolgáltatás, amely a kockázati eseményekről és a szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja. Azonosító adatok védelmét a meglévő Azure Active Directory anomáliadetektálási az észlelési képességek (az Azure AD rendellenes Tevékenységjelentések keresztül érhető el) használ, és vezet be az új kockázat típusait, amely valós idejű rendellenességek észlelését.

## <a name="secured-resource-access-in-azure"></a>Védett erőforrások elérése az Azure-ban

Hozzáférés-vezérlés az Azure-ban elindul egy számlázási szempontjából. Azure-fiók esetén érhető el hello felkeresésével hello tulajdonosának [Azure Accounts Center](https://account.windowsazure.com/subscriptions), hello AA fiók rendszergazdai. Előfizetések számlázási tárolója, de úgy is működnek, mint a biztonsági határ: minden előfizetés rendelkezik a szolgáltatás rendszergazdai (SA) akik hozzáadása, eltávolítása és módosítása az Azure-erőforrások az adott előfizetés hello használatával [a klasszikus Azure portálon](https://manage.windowsazure.com/). az új előfizetés hello alapértelmezett SA hello AA, de hello AA módosíthatja a hello Azure Accounts Center hello SA.

![Védett erőforrások elérése az Azure-ban](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Előfizetések is könyvtár van társítva. hello directory egy felhasználók csoportját határozza meg. Ezek a felhasználók hello munkahelyi vagy iskolai hello directory létrehozott lehetnek, vagy külső felhasználók számára (Ez azt jelenti, hogy a Microsoft Accounts). Az előfizetések által hozzárendelt szolgáltatás-rendszergazdai (SA) vagy Társadminisztrátorának (CA); directory felhasználók egy részhalmazát érhetők el hello csak egyetlen kivételt jelenti, hogy az örökölt összetevők miatt, Microsoft Accounts (korábbi nevén Windows Live ID Azonosítóval) rendelhető rendszergazdai (SA) vagy a CA anélkül, hogy jelen hello könyvtárban.

A vállalat biztonsági célú hello pontos engedélyeket szükségük ad az alkalmazottak kell összpontosítania. Túl sok engedélyek egy fiók tooattackers is elérhetővé teheti. Túl kevés engedélyek jelenti azt, hogy az alkalmazottak nem munkavégzéséhez hatékony. [Azure szerepköralapú hozzáférés-vezérlés (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) nyújt segítséget nyújtó részletes hozzáféréskezelést az Azure által oldja meg a problémát.

![Védett erőforrásokhoz való hozzáférést ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

Szerepalapú használ, feladatokat elkülönítse a munkacsoporton belül, és hozzáférési toousers csak hello összegét adja meg, hogy be kell tooperform a munkájukat. Jogosultságot ad a Mindenki helyett korlátozás engedélyek az Ön Azure-előfizetés vagy erőforrásokhoz, engedélyezheti a csak bizonyos műveleteket. Például használja az RBAC toolet egy alkalmazott egy előfizetésben található virtuális gépek kezeléséhez, miközben egy másik kezelhető SQL-adatbázisok hello belül azonos előfizetéssel.

![Azure(RBAC) a védett erőforrások elérése](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Az Azure data biztonsági és a titkosítás (védelem)

Egy hello kulcsok toodata védelmi hello felhőben van elszámolása hello lehetséges állapota, amelyben az adatok akkor fordulhat elő, és milyen vezérlők érhetők el az adott állapotban. Az adatok az Azure biztonsági és a titkosításhoz ajánlott eljárásait hello kell a következő adatok állapotok hello körül.

- Nyugalmi: Ez magában foglalja a tárolási objektum, a tárolók és a fizikai adathordozó statikusan létező típusok kell azt mágneses vagy optikai lemez összes információt.

- Az átvitel közbeni: Amikor adatátvitel közötti összetevők, a helyek vagy a programok, például hello hálózaton keresztül egy service bus (a helyszíni toocloud, és fordítva, beleértve a hibrid kapcsolatok, például az ExpressRoute) keresztül vagy egy bemeneti/kimeneti során folyamat, hogy van-re, hogy a mozgásérzékelési.

### <a name="encryption--rest"></a>Titkosítási rest @

tooachieve titkosítását minden hello alábbi:

Legalább egy titkosítási modellek részletes leírást talál a következő tábla tooencrypt adatok hello ajánlott hello támogatja.

| Titkosítási modellek |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Kiszolgáló-titkosítás | Kiszolgáló-titkosítás | Kiszolgáló-titkosítás | Ügyfél-titkosítás
| Kiszolgálóoldali titkosítás segítségével felügyelt kulcsai | Kiszolgálóoldali titkosítás Customer-Managed kulcsok Azure Key Vault használatával | Kiszolgálóoldali titkosítás a helyszínen felügyelt ügyfél kulcsok használata |
| • Az azure erőforrás-szolgáltatók hello titkosítási és visszafejtési műveleteket végezhet. <br> • A Microsoft hello kulcsok kezelése <br>• Teljes felhő funkció | • Az azure erőforrás-szolgáltatók hello titkosítási és visszafejtési műveleteket végezhet.<br>• Felhasználói vezérlők kulcsok Azure Key Vault keresztül<br>• Teljes felhő funkció | • Az azure erőforrás-szolgáltatók hello titkosítási és visszafejtési műveleteket végezhet. <br>• Felhasználói vezérlők helyszíni kulcsok <br> • Teljes felhő funkció| • Azure-szolgáltatások visszafejtett adatait nem látja. <br>• Ügyfelek megőrizheti a kulcsokat a helyi (vagy más biztonságos tárolók). Kulcsokban nincs elérhető tooAzure szolgáltatások <br>• Csökkenteni felhő funkció|

### <a name="enabling-encryption-at-rest"></a>Inaktív titkosítás engedélyezése

**Az összes hely a tároló adatok azonosítása**

hello cél titkosítási aktívan tooencrypt az összes adatot. Ezzel megszünteti a hiányzó fontos adatokat, vagy az összes megőrzött helyét hello lehetőségét. Az alkalmazás által tárolt összes adat számbavétele. 

> [!Note] 
> Nem csak "alkalmazásadatok" vagy "PII", de tooapplication beleértve a vonatkozó adatok fiók metaadatok (előfizetés hozzárendelések, szerződésadatok, PII).

Fontolja meg, mi tárolja, hogy toostore adatokat használ. Példa:

- A külső tárhelyen (például az SQL Azure Document DB rendszerbe, HDInsights, Data Lake, stb.)

- Ideiglenes tárhely (bármely helyi gyorsítótár, amely tartalmazza a bérlő adatok)

- A memória gyorsítótárának (léptethetnek be hello oldal fájlja.)

### <a name="leverage-hello-existing-encryption-at-rest-support-in-azure"></a>Kihasználja a meglévő titkosítását hello rest-támogatás az Azure-ban

A különféle áruházakból használja használja a titkosítási érvényben lévő többi támogatási hello ki.

- Az Azure Storage: Lásd: [az inaktív adatok Azure Storage szolgáltatás titkosítási](https://docs.microsoft.com/azure/storage/storage-service-encryption),

- Az SQL Azure: Lásd: [átlátható adattitkosítás (TDE) mindig titkosítja SQL](https://msdn.microsoft.com/library/mt163865.aspx)

- Virtuális gép & helyi lemez tárolási ([Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

A virtuális gép és a helyi lemez tárolásához használja az Azure Disk Encryption ahol támogatott:

IaaS

A szolgáltatásokat, az infrastruktúra-szolgáltatási virtuális gép (Windows vagy Linux) kell használnia [Azure Disk Encryption](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) ügyféladatok tartalmazó tooencrypt köteteket.

A PaaS v2

Futó szolgáltatások a PaaS v2 a Service Fabric használatával Azure disk encryption a virtuálisgép-méretezési csoport [VMSS] tooencrypt a PaaS v2 virtuális gép.

A PaaS 1-es verzió

Az Azure Disk Encryption jelenleg nem támogatott a PaaS 1-es verzió. Ezért az alkalmazás szintjén használnia kell titkosítási tooencrypt tárolt adatok aktívan.  Ez magában foglalja, de nem korlátozódik az alkalmazásadatok, ideiglenes fájlok, naplók és összeomlási memóriaképeket.

A legtöbb szolgáltatások arányosan tooleverage hello titkosítását a storage erőforrás-szolgáltató. Egyes szolgáltatások toodo explicit titkosítási rendelkeznek, például a megőrzött kulcsokat tárol (tanúsítványok, a legfelső szintű / fő kulcsok) Key Vault kell tárolni.

Ha támogatja a kiszolgálóoldali titkosítást ügyfél által felügyelt kulcsokkal kell toobe úgy hello ügyfél tooget hello kulcs toous. hello támogatott, és javasolt módja toodo integrálja az Azure Key Vault (AKV). Ebben az esetben az ügyfelek hozzáadása és kezelése az Azure Key Vault a kulcsok. Az ügyfél megtudhatja, hogyan toouse AKV keresztül [Ismerkedés a Key Vault](http://go.microsoft.com/fwlink/?linkid=521402).

az Azure Key Vault toointegrate, akkor hozzáadhat kód toorequest kulcs AKV visszafejtéshez szükség esetén.

- Lásd: [Azure Key Vault – részletes](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) kapcsolatos információk az AKV toointegrate.

Ha támogatja az ügyfél által felügyelt kulcsok, szükség van egy UX tooprovide hello ügyfél toospecify mely Key Vault (vagy a Key Vault URI) toouse.

Aktívan titkosításnál hello állomás, az infrastrukturális és bérlői adatok titkosítását, mivel hello kulcsok megfelelő toosystem hiba vagy rosszindulatú tevékenység megszűnését hello összes hello titkosított adatok nem vesztek el azt is jelentheti. Éppen ezért fontos, hogy rendelkezik-e a titkosítás Rest megoldást egy átfogó vészhelyreállítás szövegegység rugalmas toosystem hibák és a rosszindulatú tevékenységhez.

Szolgáltatások, amelyek megvalósítják az aktívan titkosítása általában továbbra is ki vannak téve toohello titkosítási kulcsok vagy hagyja adatok titkosítás nélkül hello gazdameghajtó (például a hello oldal fájlja hello gazda operációs rendszer.) Ezért szolgáltatások biztosítania kell hello gazdakötet szolgáltatásaik titkosítva van. toofacilitate a számítási csapat engedélyezve van a gazdagép titkosítási, amely használja hello telepítési [Bitlocker](https://technet.microsoft.com/library/dn306081.aspx) NKP és bővítmények toohello DCM szolgáltatás és az ügynök tooencrypt hello gazdakötet.

A legtöbb szolgáltatások szabványos Azure virtuális gépeken van megvalósítva. Ezek a szolgáltatások kapja meg [állomás titkosítási](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) automatikusan amikor számítási lehetővé teszi. Tartozó számítási szolgáltatások felügyelt fürtöt állomás titkosítási automatikusan engedélyezve lesz, Windows Server 2016 lesz állítva.

### <a name="encryption-in-transit"></a>Titkosítás az átvitel

Az átvitel során az adatok védelme a data protection stratégia nagyon fontos részét kell lennie. Adatok áthelyezése több helyről oda-vissza van, mert a hello általános javaslat, hogy mindig használjon SSL/TLS protokollok tooexchange adatok különböző helyek között. Bizonyos esetekben érdemes lehet tooisolate hello teljes kommunikációs csatornát a helyszíni és a felhő közötti virtuális magánhálózati (VPN) segítségével infrastruktúra.

Az adatok áthelyezése a helyszíni infrastruktúra és az Azure között megfelelő védelmi funkciók, például a HTTPS- vagy VPN-érdemes lehet.

Használja a szervezet számára több munkaállomások helyszíni tooAzure toosecure hozzáférést igénylő [Azure telephelyek közötti VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Olyan szervezeteknek, amelyek egy munkaállomásról toosecure hozzáférésre van szükségük a helyszíni tooAzure, használjon található [pont-pont VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Nagyobb, mint egy dedikált nagy sebességű WAN-kapcsolaton keresztül anélkül áthelyezhetők [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Ha toouse ExpressRoute, hello adatok hello alkalmazásszintű használatával is titkosíthatók [SSL/TLS](https://support.microsoft.com/kb/257591) vagy egyéb protokollok hozzáadott védelemre.

Interakció Azure Storage hello Azure portálon keresztül, ha minden tranzakciója történik, HTTPS használatával. [Storage REST API felülete](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS-KAPCSOLATON keresztül is lehet a használt toointeract [Azure Storage](https://azure.microsoft.com/services/storage/) és [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

A szervezeteknek, amelyek sikertelen tooprotect adatokat átvitel közben is jobban ki vannak téve a [-átjárójának](https://technet.microsoft.com/library/gg195821.aspx), [lehallgatás](https://technet.microsoft.com/library/gg195641.aspx), és a munkamenet-eltérítés. Ilyen támadások hello első lépéseként való hozzáférés tooconfidential adatok is lehetnek.

További Azure VPN lehetőségekről hello cikk olvasásával [tervezése és kialakítása VPN-átjáró](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Fájl szintű adatok titkosításának kényszerítése

[Az Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) által használt titkosítási, identitáskezelési és engedélyezési házirendek toohelp a fájlok és e-mailek biztonságos. Több eszközön működik az Azure RMS-telefonokon, táblagépeken és számítógépeken megvédi a szervezeten belül, mind a szervezeten kívülről. Ez a lehetőség azért lehetséges, mert az Azure RMS, ad hozzá egy védelmi szintet – hello adatok maradnak, még akkor is, ha elhagyják a szervezet területét.

Azure RMS tooprotect használatakor a fájlok szabványos kriptográfia használ a teljes körű támogatása [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). A data protection is használja az Azure RMS, ha akkor is, ha másolt toostorage, amely nincs hello vezérlése alatt van, amely hello védelme mindig fennmarad hello fájl hello megbízhatósági informatikai, például egy felhőalapú tárolási szolgáltatásba. hello azonos fordul elő mailben megosztott fájlok, hello fájl védett mellékletet tooan e-mail üzenetben, utasításokkal hogyan tooopen hello melléklet védett.
Ha Azure RMS bevezetésének megtervezése hello következőket javasoljuk:

- Telepítse a hello [RMS-megosztó alkalmazás](https://technet.microsoft.com/library/dn339006.aspx). Ez az alkalmazás integrálja az Office-alkalmazások telepítésekor egy Office-bővítmény, hogy a felhasználók egyszerűen védhetik a fájlok közvetlenül.

- Alkalmazások és szolgáltatások toosupport Azure RMS konfigurálása

- Hozzon létre [egyéni sablonok](https://technet.microsoft.com/library/dn642472.aspx) , amely az üzleti igényeknek megfelelően. Például: felső titkos adatok, az összes felső titkos alkalmazni kívánt sablont kapcsolódó e-maileket.

A szervezetek, amelyek a gyenge [adatbesorolást](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) és lehet, hogy a fájlvédelem jobban ki vannak téve toodata adatszivárgást. Megfelelő fájl védelem nélkül szervezetek nem fog tudni tooobtain üzleti elemzéseket, visszaélés-figyelő és meg kell akadályozni rosszindulatú hozzáférésektől toofiles.

> [!Note]
> További tudnivalók az Azure RMS által hello cikk elolvasása [Ismerkedés az Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).

## <a name="secure-your-application-protect"></a>Az alkalmazás biztonságos (védelem)
Bár az Azure hello infrastruktúra és az alkalmazás fut, a platform történő biztonságba helyezéséért felelős, ez a beállítás a felelősség toosecure magának az alkalmazásnak. Más szóval szüksége toodevelop, telepítését és kezelését az alkalmazás kódjában és a tartalom biztonságos módon. E nélkül az alkalmazás kódja vagy a tartalom továbbra is történhet sebezhető toothreats.

### <a name="web-application-firewall-waf"></a>Webalkalmazási tűzfal (WAF)
[Webalkalmazási tűzfal (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) szolgáltatása [Alkalmazásátjáró](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) , amely a webalkalmazások a gyakori biztonsági réseket és biztonsági rések központosított védelmet biztosít.

Webalkalmazási tűzfal hello szabályok alapján [OWASP core szabálykészletek](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 vagy a program 2.2.9-es. A webalkalmazások egyre inkább ki vannak téve rosszindulatú támadásoknak, amelyek az ismert biztonsági réseket használják ki. A biztonsági rések között közös SQL injektálási támadások, helyközi scripting támadások tooname néhány. Megakadályozza az ilyen jellegű támadások alkalmazáskód kihívást jelenthet, és előfordulhat, hogy szigorú karbantartása, javítását és ellenőrzés hello alkalmazás topológia több réteget. Központosított webalkalmazási tűzfal segít tisztázni biztonságkezelés jóval egyszerűbb, és lehetővé teszi a nagyobb megbízhatósági tooapplication rendszergazdák fenyegetések és a behatolás elleni. WAF megoldás is reagálhasson tooa biztonsági kockázatot jelentenek gyorsabb által egy ismert biztonsági rések egy központi helyen, és minden egyes webalkalmazás biztonságossá tétele érdekében. Meglévő alkalmazásátjárót könnyen lehet konvertált tooa webes alkalmazás engedélyezve van a tűzfal Alkalmazásátjáró.

Néhány hello közös webes biztonsági rések mely webalkalmazási tűzfal véd tartalmazza:

- SQL-injektálás elleni védelem

- Webhelyek közötti, parancsprogramot alkalmazó támadások elleni védelem

- Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem

- HTTP protokoll megsértése elleni védelem

- HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem

- Robotprogramok, webbejárók és képolvasók elleni védelem

- A gyakori alkalmazás konfigurációs hibák (Ez azt jelenti, hogy Apache, az IIS, stb.) észlelése

> [!Note]
> További szabályok részletes listáját és azok védelmét: hello következő [szabálykészletek alapvető](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure is biztosít több könnyen használható szolgáltatások toohelp biztosítják a bejövő és kimenő forgalmát az alkalmazásra vonatkozóan. Azure is segítséget nyújt az ügyfelek az alkalmazás kódjában biztonságos azáltal, hogy külsőleg biztosított funkció tooscan a webes alkalmazás a biztonsági réseket.

- [Tegye biztonságossá webalkalmazását, hitelesítési és engedélyezési különböző eszközök használatával](https://docs.microsoft.com/azure/app-service-web/web-sites-authentication-authorization)

    - [Az alkalmazás Azure Active Directory-hitelesítés beállítása](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)


- [Biztonságos forgalom tooyour app Transport Layer Security (TLS/SSL) - HTTPS engedélyezése](https://docs.microsoft.com/azure/app-service-web/web-sites-configure-ssl-certificate)

    - [Minden bejövő forgalom kényszerítheti a HTTPS-kapcsolaton keresztül](http://microsoftazurewebsitescheatsheet.info/)

  - [Szigorú a Transport Security (HSTS) engedélyezése](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)


- [Ügyfél IP-cím szerint access tooyour alkalmazásnak korlátozása](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Ügyfél viselkedése - kérelem gyakoriságának és az egyidejű hozzáférés tooyour app korlátozása](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [A webes alkalmazás kódja a Tinfoil Security Scanning eszköz használatával biztonsági réseket vizsgálata](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [A TLS kölcsönös hitelesítés toorequire ügyfél tanúsítványok tooconnect tooyour webalkalmazás konfigurálása](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth)

- [Ügyfél-tanúsítvány konfigurálása az alkalmazás toosecurely a csatlakozási tooexternal erőforrások](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Általános jogú kiszolgálói fejlécek tooavoid eszközök eltávolítása az alkalmazás-ujjlenyomat](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Az alkalmazás biztonságos kapcsolódni egy pont – hely típusú VPN használatával magánhálózati erőforrásokat](https://docs.microsoft.com/azure/app-service-web/web-sites-integrate-with-vnet)

- [Az alkalmazás biztonságos kapcsolódni az erőforrásokkal egy magánhálózaton hibrid kapcsolatok használata](https://docs.microsoft.com/azure/app-service-web/web-sites-hybrid-connection-get-started)

Az Azure App Service által használt hello ugyanaz a kártevővédelmi megoldás Azure Cloud Services és a virtuális gépek által használt. További információ toolearn tekintse meg a tooour [kártevőirtó dokumentáció](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>A hálózat biztonságáról (védelem)
Microsoft Azure tartalmaz egy robusztus hálózati infrastruktúra toosupport, az alkalmazás és szolgáltatás hálózati kapcsolati követelményeinek. Hálózati kapcsolat lehetséges helyszíni között, az Azure-ban lévő erőforrások között, és Azure megtalálható erőforrásokhoz, és az hello Internet tooand és az Azure.

Hello [Azure hálózati infrastruktúra](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) lehetővé teszi, hogy Ön toosecurely csatlakozzon a többi Azure-erőforrások tooeach [virtuális hálózatokról (Vnetekről)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). A virtuális hálózat a saját hálózati hello felhőben megjelenítése. Egy virtuális hálózat a logikai elkülönítés hello Azure felhőben dedikált hálózati tooyour előfizetés. Vnetek tooyour helyszíni hálózatokhoz is elérheti.

![A hálózat biztonságáról (védelem)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Ha szüksége alapvető hálózati szintű hozzáférés-vezérlés (IP-cím és hello TCP vagy UDP protokoll alapján), akkor is használhatja [hálózati biztonsági csoportok](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg). A hálózati biztonsági csoport (NSG) egy tűzfal alapszintű csomag és toocontrol hozzáférést alapján lehetővé teszi egy [5 rekordos](https://www.techopedia.com/definition/28190/5-tuple).

Azure hálózatkezelés hello képességét toocustomize hello útválasztási viselkedés támogatja az Azure virtuális hálózat hálózati forgalmát. Ehhez konfigurálásával [felhaszn útvonalak](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) az Azure-ban.

[A kényszerített bújtatás](https://www.petri.com/azure-forced-tunneling) egy olyan mechanizmus, amely a szolgáltatások nincsenek tooensure használható egy kapcsolat toodevices engedélyezett tooinitiate hello Internet.

Az Azure által támogatott dedikált WAN-kapcsolaton kapcsolat tooyour a helyszíni hálózat és egy Azure virtuális hálózat [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). hello hivatkozás Azure és a webhely között, amelyek nem halad át hello dedikált kapcsolatot használ nyilvános internethez. Ha több adatközpontot az Azure alkalmazás fut, akkor használhatja [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) tooroute kérelmeinek intelligens módon különböző hello alkalmazás példányát. Nem fut az Azure-ban, ha azok elérhetők a hello internetes forgalom tooservices is irányíthatja.

## <a name="virtual-machine-security-protect"></a>Virtuális gép biztonsági (védelem)

[Az Azure virtuális gépek](https://docs.microsoft.com/azure/virtual-machines/) lehetővé teszi, hogy számos számítógép-használatról megoldások egy gyors módja a telepítése. Számítási feladatait a Windows-, Linux-, SQL Server-, Oracle-, IBM-, SAP- és Azure BizTalk Services-támogatás révén mérettől és programnyelvtől függetlenül, szinte bármely operációs rendszerben üzembe helyezheti.

Használhatja az Azure- [kártevőirtó szoftver](https://docs.microsoft.com/azure/security/azure-security-antimalware) biztonsági szállítók, például a Microsoft, illetve Symantec, Trend Micro és Kaspersky tooprotect a virtuális gépek rosszindulatú fájlok, hirdetéseket és más fenyegetésekkel szemben.

Azure Cloud Services és a virtuális gépek Microsoft Antimalware a valós idejű védelem képessége, amelyik segít azonosításához és eltávolításához a vírusok, kémprogramok és más, kártevő szoftverek. A Microsoft Antimalware konfigurálható riasztást küld, ha ismert kártevő vagy nemkívánatos szoftver kísérletek tooinstall magát, vagy az Azure rendszeren futtatásához biztosít.

[Azure biztonsági mentés](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) van méretezhető megoldás, amely megvédi az alkalmazásadatok nulla tőkebefektetés szükségessége, és minimális üzemeltetési költségek. Az alkalmazáshibák adatai sérülését okozhatják, az emberi hibák pedig az alkalmazások meghibásodásához vezethetnek. Az Azure Backup szolgáltatással a Windows és Linux rendszerű virtuális gépek védelmének.

[Az Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) segít replikáció, feladatátvétel és helyreállítás munkaterhelések és alkalmazások téve, hogy azok elérhetők a másodlagos helyről, ha az elsődleges hely leáll.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>A megfelelőség biztosítása: a felhőalapú szolgáltatások megfelelő gondossággal ellenőrzőlista (védelem)

Microsoft fejlesztett [felhőalapú szolgáltatások esedékes gondossággal ellenőrzőlista hello](https://aka.ms/cloudchecklist.download) toohelp szervezetek gyakorolja miatt gondossággal szerint áthelyezés toohello felhő. Szerkezetet biztosít bármely méretét, és írja be a szervezet – titkos üzleteknek és nyilvános szektorokkal rendelkező szervezeteknek, többek között a szintek és a nonprofit kormányzati – tooidentify saját teljesítmény, a szolgáltatás, a adatok kezelése és a cégirányítási célkitűzések és követelmények. Ez lehetővé teszi őket a szolgáltatók másik felhőt, végső soron a egy felhőalapú szolgáltatási szerződés hello alapját képező toocompare hello ajánlatokat.

hello ellenőrzőlista keretrendszerében záradék-által-záradékot egy új nemzetközi szabványa felhőalapú szolgáltatási szerződések, ISO/IEC 19086 igazodik. Ezzel a szabvánnyal szempontjai a felhő elfogadása kapcsolatos döntések, és hozzon létre egy közös ground felhő szolgáltatásajánlatok összehasonlítása a szervezetek toohelp látványelemek kínál.

hello ellenőrzőlista elősegíthető alaposan vetted áthelyezés toohello felhő, így strukturált útmutatást és egy egységes, ismételhető megközelítést felhő szolgáltatót.

Felhő elfogadása most már egyszerűen technológia döntést hoznak. Egy szervezet minden szempontját érintik ellenőrzőlista követelményeinek, mert ezek szolgálnak tooconvene minden kulcs belső – döntéshozók – hello CIO és adatvédelmi felelős, valamint jogi, kockázati kezelési, a beszerzési és a megfelelőségi szakemberek számára. Ez növeli a hello döntéshozatali folyamat és a hang mintafelismerési, ezáltal csökkenti a hello annak valószínűségét, hogy előre nem látható roadblocks tooadoption ground szempontjait hello hatékonyságát.

Ezenkívül hello Ellenőrzőlista:

- Elérhetővé teszi a fő ismertető témakörök a döntéshozók hello elején hello felhő bevezetésének folyamatát.

- Alapos üzleti beszélgetéseket szabályozások és hello szervezete saját célok támogatja az adatok biztonsági, adatvédelmi és személyes azonosításra alkalmas adatokat (PII).

- Segítségével a szervezetek azonosítani a potenciális problémákat, amely hatással lehet egy felhőalapú projektet.

- Hello, a kérdéseket a konzisztens biztosít azonos feltételeket, definíciókat, metrikákat és termékek esetében az egyes szolgáltatók toosimplify hello folyamata a különböző felhőszolgáltatók ajánlatok összehasonlítása.

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Azure infrastruktúra és az alkalmazás biztonsági ellenőrzés (észlelni)

[Az Azure Operational biztonsági](https://docs.microsoft.com/azure/security/azure-operational-security) toohello szolgáltatások, a vezérlők és a szolgáltatások rendelkezésre álló toousers hivatkozik az adatok, alkalmazások és egyéb eszközök a Microsoft Azure-ban védelmére.

![biztonsági érvényesítési (észlelni)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Az Azure Operational biztonsági épül egy keretrendszer, amely magában foglalja a hello tapasztalatok teszik a különböző képességeket, amelyek egyedi tooMicrosoft, beleértve a Microsoft biztonságos fejlesztési Életciklussal (SDL), a Microsoft biztonsági válasz Center hello hello keresztül program, és részletes tájékoztatást nyújthatnak a hello számítógépes fenyegetésekről alkotott képet.

### <a name="microsoft-operations-management-suiteoms"></a>A Microsoft operations management suite(OMS)

[A Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) hello hello hibrid felhőalapú informatikai felügyeleti megoldás. Használható önmagában, vagy a meglévő System Center telepítés esetében az OMS lehetővé teszi az tooextend hello maximális rugalmasságot és a felhő alapú felügyeleti infrastruktúra.

![A Microsoft operations management suite(OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

Az OMS-ben a felhő, beleértve a helyszíni, Azure, AWS, Windows Server, Linux, VMware és OpenStack, mint versenyképes megoldások alacsonyabb költségekkel szereplő bármely példány kezelheti. Felhő-első world hello készült, OMS biztosít egy új módszer toomanaging hello lehető leggyorsabb és legköltséghatékonyabb módon toomeet új üzleti felkéri, és új munkaterhelések, alkalmazások és a felhő környezeteiben megfeleljen a vállalat.

### <a name="log-analytics"></a>Log Analytics

A [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) figyelési szolgáltatásokat biztosít az OMS számára a felügyelt erőforrások adatainak egy központi tárházba gyűjtésével. Ezek az adatok tartalmazhatnak eseményeket, teljesítményadatokat vagy egyéni hello API keresztül elérhető adatok. Amennyiben az összegyűjtött, riasztási, elemzés és exportálási hello adatok érhető el.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Ez a módszer lehetővé teszi különböző forrásokból tooconsolidate adatait, így kombinálhatja az Azure-szolgáltatások és a meglévő adatokat a helyszíni környezet. Azt is egyértelműen elválasztja hello adatgyűjtés hello hello műveletet végre az adatok, így minden elérhető tooall típusú adatok.

### <a name="azure-security-center"></a>Az Azure security Centerben

[Az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) és nyújt segítséget megakadályozása, észleli, és a láthatóság növelésével toothreats válaszolni vezérlése hello Azure-erőforrások biztonsági. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

A Security Center elemzi az Azure-erőforrások tooidentify potenciális biztonsági hiányosságok hello biztonsági állapotát. A javaslatok listája végigvezeti hello szükséges szabályozási konfigurálásának lépésein.

Példák erre vonatkozóan:

- Üzembe helyezési kártevőirtó toohelp azonosítása és eltávolítani a kártevő szoftvereket

- Hálózati biztonsági csoportok és a szabályok toocontrol forgalom tooVMs konfigurálása

- A webes alkalmazások célzó támadások elleni védelemre toohelp webalkalmazási tűzfalak kiépítése

- Hiányzó rendszerfrissítések telepítése

- Operációs rendszer azon konfigurációit, amelyek nem felelnek meg a hello címzési ajánlott alaptervek

A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, a hello hálózati és a partneri megoldások, például kártevőirtó-programok és tűzfalak naplóadatait. Fenyegetések észlelése esetén a központ biztonsági riasztást hoz létre. Példák fenyegetés észlelésére:

- Feltört virtuális gépek kommunikál az ismert kártékony IP-címek

- Windows hibajelentés által észlelt speciális kártevő

- Virtuális gépek elleni találgatásos támadások ellen

- Az integrált kártevőirtó programok és a tűzfalak biztonsági riasztásai

### <a name="azure-monitor"></a>Az Azure-figyelő

[Az Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) mutatók tooinformation biztosít az adott típusú erőforrás. A képi megjelenítés, lekérdezés, útválasztási, riasztás, automatikus méretezési, és kínál automation adatait mindkét hello Azure-infrastruktúra (tevékenységnapló) és minden egyes Azure-erőforrás (diagnosztikai naplók).

Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz. Figyelési adatok tooensure, hogy az alkalmazás be és a megfelelő állapotban fut biztosít. Emellett segít, toostave ki a lehetséges problémák és a múltbeli kiépítettektől eltérő hibakeresést.

![Az Azure figyelő](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) emellett használhatja a figyelési adatok toogain mélyebben elemezheti az alkalmazással kapcsolatban. Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.

A hálózati biztonsági naplózás létfontosságú a hálózati biztonsági rések észlelése és az IT-biztonsági és szabályozási irányítás modell betartását. Biztonsági csoport nézet akkor is hello konfigurált hálózati biztonsági csoport és a biztonsági szabályok lekérése, valamint hello hatékony biztonsági szabályokat. Szabályok alkalmazása hello listája azt is meghatározhatja megnyitott portok hello és ss hálózati biztonsági rések.

### <a name="network-watcher"></a>Hálózati figyelőt

[Hálózati figyelő](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) regionális szolgáltatás, amely lehetővé teszi a toomonitor és diagnosztizálhatja az, hogy és az Azure-ból hálózati szintű feltételeket. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban. A szolgáltatás része a csomagrögzítéssel, a következő ugrás, az IP-adatfolyam győződjön meg arról, biztonsági csoport megtekintése, NSG folyamata naplókat. Forgatókönyv szintű figyelési jeleníti meg egy záró tooend ezzel szemben tooindividual hálózati erőforrás figyelési hálózati erőforrásokhoz.

### <a name="storage-analytics"></a>Storage Analytics

[Tárolási analitika](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) tárolhatja, amely tartalmazza az összevont tranzakció statisztikák és a kapacitás adatok kérelmek tooa storage szolgáltatással kapcsolatos metrikákat. Tranzakciók jelentett mindkét hello API művelet szinten, továbbá hello tárolási szolgáltatás szintjén, és a kapacitás hello tárolási szolgáltatás szintjén jelenti. Metrikai adatok is használt tooanalyze szolgáltatás tárhelyhasználatot, eseményadatokat az felé irányuló hello társzolgáltatás és tooimprove hello teljesítményét egy szolgáltatást használó alkalmazások által.

### <a name="application-insights"></a>Az Application insights

[Az Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) egy bővíthető alkalmazásteljesítmény-felügyeleti (APM) szolgáltatás webfejlesztőknek, több platformon. Ezzel toomonitor élő webalkalmazásokat. Automatikusan felismeri a teljesítményanomáliákat. Ez magában foglalja a hatékony analytics eszközök toohelp problémákat és a felhasználók milyen elvégezni az alkalmazás toounderstand diagnosztizálásához. Úgy van kialakítva, hogy folyamatosan teljesítményük és használhatóságuk javításában toohelp. Az alkalmazások működését platformokon, beleértve a .NET, Node.js és J2EE számos, a helyben tárolt vagy hello felhőben. Integrálható a devOps folyamat, és a csatlakozási pontok tooa különböző Fejlesztőeszközök rendelkezik.

A szolgáltatás az alábbiakat figyeli:

- **Kérések sebessége, válaszidők és hibaarányok** – megtudhatja, hogy mely lapok, mely napszakokban a legnépszerűbbek, és hol találhatók a felhasználók. Megtekintheti, hogy mely lapok teljesítenek a legjobban. Ha több kérés esetén a válaszidők és a hibaarányok értéke megnő, valószínűleg erőforrás-gazdálkodási hibáról van szó.

- **Függőségi értékek, válaszidők és hibaarányok** – megtudhatja, hogy mely külső szolgáltatások okoznak lassulást.

- **Kivételek** - elemzése hello összesített statisztikák, vagy válasszon olyan specifikus példányai, és elemezze a hello veremkiíratási adataival és a kapcsolódó kérések. A kiszolgálói és a böngészői kivételekről egyaránt készül jelentés.

- **Lapmegtekintések és betöltési teljesítmény** – a felhasználói böngészők jelentése alapján készül.

- **AJAX-hívások weblapokról** -sebességét, a válaszidőt és a hiba sebességét.

- **Felhasználó és a munkamenet számát.**

- Windows vagy Linux rendszerű kiszolgálói gépekről származó **teljesítményszámlálók**, például processzor-, memória- és hálózathasználat.

- Dockerből vagy Azure-ból származó **gazdadiagnosztika**.

- Alkalmazásból származó **nyomkövetési naplók diagnosztikája** – megállapíthatja a nyomkövetési események és a kérések korrelációját.

- **Egyéni események és metrikák** hogy írni saját kezűleg hello ügyfél vagy kiszolgáló kód, például értékesített cikkek tootrack üzleti események vagy nyert játékok.
az alkalmazás hello infrastruktúrája általában számos összetevőből áll – például egy virtuális gépből, tárfiókot, és virtuális hálózati vagy egy webalkalmazást, adatbázis, adatbázis-kiszolgáló és 3. fél szolgáltatások épül fel. Ezeket az összetevőket nem külön entitásokként látja, hanem egyetlen entitás kapcsolódó és egymással összefüggő részeiként. Szeretné, hogy toodeploy, kezelheti és figyelheti azokat csoportként. [Az Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) toowork hello erőforrásokat csoportként a megoldás lehetővé teszi.

Telepíteni, frissíteni vagy hello összes erőforrást törli a megoldás egyetlen, koordinált műveletben. A telepítéshez egy sablon használatos, amely különböző, például tesztelési, átmeneti és üzemi környezetben is képes működni. A Resource Manager biztosít biztonsági, naplózási és címkézési szolgáltatásokat toohelp, kezelheti az erőforrásokat a telepítés utáni.

**Erőforrás-kezelő használatával hello előnyei**

A Resource Manager számos előnyt kínál:

- Telepíthetik, felügyelhetik és erőforrások hello figyeli, hogy a megoldás egy csoportot, hanem erőforrások különálló kezelése.

- Ismételten telepítheti a megoldást hello fejlesztési életciklus során, és lehet abban, hogy az erőforrások telepítése konzisztens lesz.

- Az infrastruktúrát szkriptek helyett deklaratív sablonok segítségével is kezelheti.

- Megadhatja, hogy hello függőségek között erőforrásokat, hogy azok hello megfelelő sorrendben legyenek telepítve.

- Hozzáférés-vezérlési tooall szolgáltatásokat alkalmazhat az erőforráscsoportban, mivel a szerepköralapú hozzáférés-vezérlést (RBAC) natív módon integrálva hello felügyeleti platform.

- Címkékkel láthatja tooresources toologically rendszerezése összes hello erőforrást az előfizetésében.

- Tisztázhatja a szervezete erőforrások csoportjának költségeinek megtekintésével megosztása hello azonos címkével.

> [!Note]
> Erőforrás-kezelő egy új módon toodeploy biztosít, és a megoldások kezelése. Ha követte hello korábbi telepítési modellt és toolearn hello változásokról, tekintse meg [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Következő lépések

Többet szeretne tudni biztonsági ehhez beolvassa a részletes biztonsági témakörök:

- [Audit és naplózás](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Kiberbűnözés mértéke](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Tervezési és a működési biztonság](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Titkosítás](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Identitás és hozzáférés-kezelés](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Hálózati biztonság](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Fenyegetések kezelése](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)
