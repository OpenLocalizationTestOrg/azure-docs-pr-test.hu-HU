---
title: "Azure Active Directory feltételes hozzáférés |} Microsoft Docs"
description: "Az Azure Active Directoryban feltételes hozzáférés-vezérlés segítségével meghatározott feltételek keresése, amikor az alkalmazás-hozzáférés hitelesítéséhez."
services: active-directory
keywords: "alkalmazások, a feltételes hozzáférés az Azure ad-vel, a biztonságos hozzáférés a vállalati erőforrásokhoz, a feltételes hozzáférési házirendekkel a feltételes hozzáférés"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 20572ecbde79bc2722f3a25f297c92d8e722a3e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Feltételes hozzáférés az Azure Active Directoryban

Mobileszköz-first, a felhő-első világában Azure Active Directory lehetővé teszi, hogy az egyszeri bejelentkezés eszközök, alkalmazások és szolgáltatások bárhonnan. (Beleértve a BYOD) eszközök elterjedésével használhatók ki a vállalati hálózathoz, és a 3. fél Szolgáltatottszoftver-alkalmazásoknál, informatikai szakemberek számára két másik célt szemben:

- A végfelhasználók számára, hogy termelékenyebben legyenek a tetszőleges helyről és időben ellenőrizniük
- A vállalati eszközök védelme bármikor

A termelékenység növelése, Azure Active Directory nyújt a vállalati eszközök eléréséhez beállítások széles köre a felhasználókat. Alkalmazáshozzáférés-kezeléshez, az Azure Active Directory lehetővé teszi annak érdekében, hogy csak *a megfelelő személyeknek* férhet hozzá az alkalmazásokhoz. Mi történik, ha szeretné jobban vezérelheti, hogy a megfelelő személyeknek érnek el bizonyos körülmények között az erőforrások? Mi történik, ha lehetősége van arra is használt kívánt még a bizonyos alkalmazásokhoz való hozzáférés letiltása a *jobb gombbal a személyek*? Például előfordulhat OK gombra, ha a megfelelő személyeknek bizonyos alkalmazásokhoz fér hozzá a megbízható hálózatból; azonban nem biztos hogy ezek az alkalmazások hozzáférjenek a nem megbízható hálózaton keresztül. Meg lehet oldani ezeket a kérdéseket a feltételes hozzáférés használatának.

Feltételes hozzáférés egy olyan képességet, az Azure Active Directory, amely lehetővé teszi a környezetében a meghatározott feltételek alapján alkalmazásokhoz való hozzáférés vezérlőit kényszerítéséhez. Szabályozza akkor vagy nagy terhelést jelent a eléréséhez további követelmények vagy letilthatja azt. Feltételes hozzáférés végrehajtásának házirendek alapul. Egy csoportházirend-alapú módszer egyszerűbbé teszi a konfigurációs felhasználói élmény, mert ez azt jelenti, hogy a hozzáférési követelmények a véleménye módja.  

Általában adja meg a hozzáférési követelmények, a következő mintát alapuló utasítások segítségével:

![vezérlő](./media/active-directory-conditional-access-azure-portal/10.png)

Ha két előfordulását lecseréli a "*ez*" valós adatokkal, például egy házirend-utasítás, amely valószínűleg ismerős lehet rendelkezik:

*Ha a alvállalkozói megpróbált hozzáférni a felhőalapú alkalmazások, amelyek nem megbízható hálózatokon, majd blokkolja a hozzáférést.*

A fenti házirend-utasítás emel ki, a teljesítmény, a feltételes hozzáférés. Amíg engedélyezheti alvállalkozói, alapvetően a felhőalkalmazások eléréséhez (**ki**), a feltételes hozzáférés is adja meg a feltételeket, amelyek alapján a hozzáférés lehetséges (**hogyan**).

Azure Active Directory feltételes hozzáférés, kontextusában

- "**Ha ez történik**" nevezik **utasítás feltétel**
- "**Majd ehhez**" nevezik **vezérlők**

![vezérlő](./media/active-directory-conditional-access-azure-portal/11.png)

Egy feltétel utasítást a vezérlőkkel kombinációja a feltételes hozzáférési házirend jelöli.

![vezérlő](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Vezérlők

A feltételes hozzáférési házirendek vezérlők meghatározása. azt, amely kell fordulhat elő, ha egy feltétel utasítás teljesítését.  
A vezérlők letiltja a hozzáférést, vagy a további követelményeknek megfelelő hozzáférést.
Amikor konfigurál egy házirendet, amely lehetővé teszi a hozzáférést, kell kiválasztania legalább egy követelmény.   

### <a name="grant-controls"></a>Támogatás vezérlők
Az Azure Active Directory a jelenlegi megvalósításától a következő grant-ellenőrzésre vonatkozó követelmények konfigurálását teszi lehetővé:

![vezérlő](./media/active-directory-conditional-access-azure-portal/05.png)

- **A multi-factor Authentication** -megkövetelheti az erős hitelesítés, a többtényezős hitelesítést. -Szolgáltatóként használhatja az Azure multi-factor Authentication vagy egy helyszíni többtényezős hitelesítési szolgáltató, Active Directory összevonási szolgáltatások (AD FS) együtt. A multi-factor authentication használatával megvédi erőforrások a egy érvényes felhasználó hitelesítő adatai hozzáférést szerzett előfordulhat, hogy jogosulatlan felhasználók férnek hozzá.

- **Megfelelő eszközökre** -feltételes hozzáférési házirendek az eszköz szinten állíthatja be. Előfordulhat, hogy állít be a házirend csak akkor engedélyezze a megfelelő, vagy a mobil eszközök a mobileszköz-kezelés a szervezet erőforrásaihoz-ban regisztrált számítógépekre. Például az eszköz megfelelőségének ellenőrzése az Intune használatával, és jelentést készít a azt az Azure AD-hez kényszerítési amikor a felhasználó próbál csatlakozni egy alkalmazáshoz. Alkalmazások és adatok védelme az Intune használatával kapcsolatos részletes útmutatóért lásd: [alkalmazások és a Microsoft Intune-nal adatainak védelme](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune). Is használhatja Intune adatvédelem elveszett vagy ellopott eszközök érvényesítését. További információkért lásd: [az adatok védelme teljes vagy szelektív törléssel a Microsoft Intune használatával](https://docs.microsoft.com/intune-classic/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

- **Tartományhoz csatlakozó eszközök** – az eszközt, és az Azure Active Directory tartományhoz a helyszíni Active Directory (AD) kell segítségével megkövetelheti. A házirend a Windows asztali számítógépek, laptopok és vállalati táblagépek vonatkozik. 

Ha több vezérlő kijelölt, is beállítható, hogy az összes lesz szükség, ha a házirend feldolgozása.

![vezérlő](./media/active-directory-conditional-access-azure-portal/06.png)

### <a name="session-controls"></a>Munkamenet-vezérlők
Munkamenet vezérlők a felhőalapú alkalmazások korlátozó tapasztalatok teszik lehetővé. A munkamenet vezérlők felhőalkalmazások érvényesíti, és további Azure AD-be a munkamenetre vonatkozó az alkalmazás által biztosított információk alapján.

![vezérlő](./media/active-directory-conditional-access-azure-portal/session-control-pic.png)

#### <a name="use-app-enforced-restrictions"></a>Használja a kényszerített alkalmazásra vonatkozó korlátozások
A vezérlő segítségével az Azure AD át az eszköz információkat a cloud app szükséges. Ez segít a cloud app, ha a felhasználó a megfelelő eszköz vagy a tartományhoz csatlakoztatott eszköz származik. Ez a vezérlő jelenleg csak a SharePoint, a cloud app támogatott. SharePoint biztosít a felhasználók egy teljes vagy korlátozott élmény attól függően, hogy az eszköz állapota az eszközinformáció használja.
Nyissa meg a SharePoint korlátozott hozzáférést igényelnek kapcsolatos további tudnivalókért [Itt](https://aka.ms/spolimitedaccessdocs).

## <a name="condition-statement"></a>Feltételutasításhoz

Az előző szakaszban vezetett támogatott beállítások letiltása, vagy az erőforrások a vezérlők űrlapon való hozzáférés korlátozása. A feltételes hozzáférési szabályzatot a feltételek, amelyeknek teljesülniük kell a vezérlők feltétel utasítás formában alkalmazandó határozhatók meg.  

A következő hozzárendelések a feltételutasításhoz az alábbiakból állhat:

![vezérlő](./media/active-directory-conditional-access-azure-portal/07.png)


- **Aki** -sok esetben érdemes a vezérlők, a felhasználók adott csoportja alkalmazandó. A feltétel utasításban definiálhat az; ehhez válassza a felhasználók és csoportok a házirend vonatkozik. Ha szükséges, explicit módon is kizárhat a felhasználók egy csoportja a házirend adásával.  
Ha a felhasználók és csoportok, a házirend vonatkozik felhasználók hatókörének meghatározása.    

    ![vezérlő](./media/active-directory-conditional-access-azure-portal/08.png)



- **Mi** -általában, vannak bizonyos alkalmazások, a védelem szempontjából, mint a többire további figyelmet igénylő környezetében futnak. Ez a beállítás befolyásolja, például alkalmazások, amelyekre a bizalmas adatokhoz való hozzáférést.
Ha felhőalapú alkalmazások, a házirend vonatkozik felhőalkalmazások hatókörének meghatározása. Ha szükséges, akkor közvetlenül is kizárhatja. utóbbi esetben az alkalmazások a házirend.

    ![vezérlő](./media/active-directory-conditional-access-azure-portal/09.png)


- **Hogyan** - mindaddig, amíg a alkalmazásokhoz való hozzáférés körülmények szabályozhatja, mert előfordulhat, hogy nincs szükség további vezérlők előíró a hogyan a felhőalapú alkalmazások vannak elérhetők a felhasználók által történik. Azonban dolgot eltérő, ha a felhőalapú alkalmazásokhoz való hozzáférés hajtja végre, például a nem megbízható hálózatokhoz, vagy nem megfelelő eszközök lehet. A feltétel utasításban bizonyos hozzáférési feltételek, amelyek további követelményei: hogyan történik a alkalmazásokhoz való hozzáférés adhat meg.

    ![Feltételek](./media/active-directory-conditional-access-azure-portal/21.png)


## <a name="conditions"></a>Feltételek

Az Azure Active Directory, a jelenlegi megvalósításától meghatározhatja feltételeinek a következő területeken:

- Bejelentkezési kockázata
- Eszközök
- Helyek
- Ügyfél-alkalmazások

![Feltételek](./media/active-directory-conditional-access-azure-portal/21.png)

### <a name="sign-in-risk"></a>Bejelentkezési kockázata

A bejelentkezési kockázata olyan objektum, amely az Azure Active Directory nyomon követésére szolgál annak valószínűsége, hogy a bejelentkezés kísérlet történt egy felhasználói fiókot jogos tulajdonosa nem lett végrehajtva. Ez az objektum (magas, közepes vagy alacsony) annak a valószínűségét nevű attribútum formában tárolja [bejelentkezési kockázati szint](active-directory-reporting-risk-events.md#risk-level). Ez az objektum bejelentkezéskor a felhasználó generál, ha az Azure Active Directory bejelentkezési kockázatok észlelt. További részletek: [Kockázatos bejelentkezések](active-directory-identityprotection.md#risky-sign-ins).  
A számított bejelentkezési kockázati szint egy feltételes hozzáférési házirendben feltételként használható. 

![Feltételek](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Eszközök

Az eszköz platformjától jellemzőek, az eszközön futó operációs rendszer:

- Android
- iOS
- Windows Phone
- Windows
- macOS (előzetes verzió). 

![Feltételek](./media/active-directory-conditional-access-azure-portal/02.png)

Meghatározhatja az eszközplatformokat, amelyeknek tartalmazza, továbbá az eszközplatformokat, amelyeken a házirend alól.  
A házirend eszközplatformok használatához előbb módosítsa a konfigurálás váltógombok **Igen**, majd válassza ki az összes vagy az egyes eszközplatformok a házirend vonatkozik. Ha az egyes eszközplatformok választja, a házirend ezekről a platformokról csak hatással van. Ebben az esetben, más támogatott platformra bejelentkezések nem érinti a házirendet.


### <a name="locations"></a>Helyek

A hely azonosítja az ügyfél és az Azure Active Directory segítségével IP-címét. Ez a feltétel meg kell ismernie kell a **helyek nevű** és **MFA megbízható IP-címek**.  

**Helyek nevű** az Azure Active Directoryban, amely lehetővé teszi a szervezetek megbízható IP-címtartományokat megjelölésére szolgáltatása. A környezetben, használhatja a észlelését, az adott környezetben elnevezett helyét [kockázati események](active-directory-reporting-risk-events.md) és a feltételes hozzáférés. Az Azure Active Directory helyek nevű konfigurálásával kapcsolatos további részletekért lásd: [az Azure Active Directory helyek nevű](active-directory-named-locations.md).

Helyek konfigurálhatja az Azure ad-ben a kapcsolódó objektum mérete korlátozza. Konfigurálhatja:
 
 - Egy olyan hellyel, amely legfeljebb 500 IP-címtartományok nevű
 - Legfeljebb egy IP-címtartomány hozzájuk rendelt 60 elnevezett helyekre (előzetes verzió) 


**Többtényezős hitelesítés a megbízható IP-címek** egyik szolgáltatása, amely lehetővé teszi a megbízható IP-címtartományok a szervezet helyi intranet képviselő adható meg a multi-factor authentication. Egy hely feltételeinek konfigurálásakor megbízható IP-címek lehetővé teszi a vállalati hálózathoz, és az egyéb helyekre kapcsolatok megkülönböztetésére. További részletekért lásd: [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  



Belefoglalhatja minden hely vagy a megbízható IP-címek és kizárhatja az összes megbízható IP-címek.

![Feltételek](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-app"></a>Ügyfélalkalmazás

Az ügyfélalkalmazás lehet az alkalmazás (webböngésző, mobilalkalmazás, asztali ügyfél), és az Azure Active Directory segítségével általános szinten, vagy kifejezetten választhatja ki az Exchange Active Sync.  
Alapszintű hitelesítés, mint például a régebbi Office-ügyfelek, amelyek nem használják a modern hitelesítést használó ügyfelek régebbi hitelesítési hivatkozik. Feltételes hozzáférés jelenleg nem támogatott a hagyományos hitelesítéssel.

![Feltételek](./media/active-directory-conditional-access-azure-portal/04.png)


## <a name="common-scenarios"></a>Gyakori forgatókönyvek

### <a name="requiring-multi-factor-authentication-for-apps"></a>Többtényezős hitelesítés megkövetelése az alkalmazások

Sok környezetben a többinél magasabb szintű védelmet igénylő alkalmazások rendelkeznek.
Ez helyzet, például az alkalmazások, amelyekre a bizalmas adatokhoz való hozzáférést.
Ha a kívánt védelmi réteget hozzá ezekhez az alkalmazásokhoz, konfigurálhatja egy feltételes hozzáférési szabályzatot, amely többtényezős hitelesítést igényel, amikor a felhasználók elérik-e ezeket az alkalmazásokat.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Többtényezős hitelesítés megkövetelése a nem megbízható hálózatokhoz való hozzáférést

Ebben a forgatókönyvben nem hasonló az előző helyzethez, mert a multi-factor authentication követelmény hozzáadása.
A fő különbség azonban ez a követelmény feltételét.  
Míg az előző példában a fókuszában sensitve adatokhoz való hozzáférés alkalmazások volt, az ebben a forgatókönyvben elsősorban megbízható helyeken.  
Ez azt jelenti lehetséges, hogy a multi-factor authentication követelmény az alkalmazások a felhasználó nem megbízható hálózaton keresztül illetéktelen.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz

Intune használ a környezetben, ha azonnal megkezdheti a feltételes hozzáférési házirendek felülete a Azure-konzolon.

Számos Intune-ügyfél számára a feltételes hozzáférés segítségével győződjön meg arról, hogy csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz. Ez azt jelenti, hogy a mobileszközök az Intune-nal beléptetett és megfelelőségi házirend követelményeknek, és, hogy a Windows rendszerű számítógépek csatlakozik egy helyszíni tartományban. A kulcs fokozása, nem kell ugyanabban a házirendben beállítása az egyes az Office 365-szolgáltatásokhoz.  Ha egy új házirendet hoz létre, konfigurálja az egyes az Office 365-alkalmazásokat, amelyek feltételes hozzáférésű védeni kíván felvenni a felhőalapú alkalmazásokat.

## <a name="next-steps"></a>Következő lépések

Ha meg szeretné ismerni a feltételes hozzáférési házirend konfigurálása tudnivalókat [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal-get-started.md).

Ha készen áll a környezet feltételes hozzáférési házirend-beállításokkal, tekintse meg a [ajánlott eljárások a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-best-practices.md). 