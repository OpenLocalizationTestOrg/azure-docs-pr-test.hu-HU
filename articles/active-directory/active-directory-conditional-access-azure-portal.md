---
title: "az Active Directory feltételes hozzáférés aaaAzure |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlés az Azure Active Directory toocheck a meghatározott feltételek hitelesítéséhez használt a hozzáférési tooapplications."
services: active-directory
keywords: "feltételes hozzáférés tooapps, feltételes hozzáférés az Azure AD-vel biztonságos hozzáférés toocompany erőforrásokat, a feltételes hozzáférési házirendek"
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
ms.openlocfilehash: 9fa8a5c3e514c032fbe3aa56f33d759485a018c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Feltételes hozzáférés az Azure Active Directoryban

Mobileszköz-first, a felhő-első világában Azure Active Directory lehetővé teszi, hogy egyszeri bejelentkezés toodevices, alkalmazásokhoz és szolgáltatásokhoz bárhonnan. Esetén (beleértve a BYOD) hello elterjedésével használhatók ki a vállalati hálózathoz, és a 3. fél Szolgáltatottszoftver-alkalmazásoknál, informatikai szakemberek számára két másik célt szemben:

- Hello végfelhasználók toobe hatékony építve, amikor és ahol csak
- Bármikor hello vállalati eszközök védelme

tooimprove hatékonyságot, Azure Active Directory biztosít a felhasználók a beállítások tooaccess széles körének a vállalati eszközöket. Alkalmazáshozzáférés-kezeléshez, az Azure Active Directory lehetővé teszi, hogy csak tooensure *hello a megfelelő személyeknek* férhet hozzá az alkalmazásokhoz. Mi történik, ha azt szeretné, toohave hogyan hello megfelelő személyeknek érnek el bizonyos körülmények között az erőforrások több ellenőrzést? Mi történik, ha lehetősége van arra is kérünk tooblock hozzáférés toocertain alkalmazások még a hello feltételek *jobb gombbal a személyek*? Például előfordulhat OK gombra, ha hello megfelelő személyeknek bizonyos alkalmazásokhoz fér hozzá a megbízható hálózatból; azonban előfordulhat, hogy nem kívánja azokat tooaccess ezeket az alkalmazásokat, nem megbízható hálózaton keresztül. Meg lehet oldani ezeket a kérdéseket a feltételes hozzáférés használatának.

Feltételes hozzáférés egy olyan képességet, az Azure Active Directoryban, amely lehetővé teszi a tooenforce vezérlőit hello hozzáférés tooapps az adott környezetben meghatározott feltételek alapján. A vezérlők, vagy nagy terhelést jelent további követelmények toohello hozzáférés, vagy letilthatja azt. hello végrehajtásának feltételes hozzáférési házirendek alapul. Egy csoportházirend-alapú módszer egyszerűbbé teszi a konfigurációs felhasználói élmény, mert ez azt jelenti, hogy a hozzáférési követelmények a véleménye hello módon.  

Általában adja meg a hozzáférési követelmények, a következő mintát hello alapuló utasítások segítségével:

![vezérlő](./media/active-directory-conditional-access-azure-portal/10.png)

Ha hello két előfordulását lecseréli a "*ez*" valós adatokkal, például egy házirend-utasítás, amely valószínűleg ismerős tooyou rendelkezik:

*Ha alvállalkozói próbált tooaccess a felhőalapú alkalmazások, amelyek nem megbízható hálózatokon, majd blokkolja a hozzáférést.*

a fenti hello házirend-utasítás a feltételes hozzáférés hello power mutatja be. Amíg engedélyezheti alvállalkozói toobasically hozzáférni a felhőalkalmazások (**ki**), a feltételes hozzáférés is meghatározhat feltételek alapján mely hello hozzáférési lehetőség (**hogyan**).

A feltételes hozzáférés Azure Active Directory, a hello környezetben

- "**Ha ez történik**" nevezik **utasítás feltétel**
- "**Majd ehhez**" nevezik **vezérlők**

![vezérlő](./media/active-directory-conditional-access-azure-portal/11.png)

egy feltétel utasítást a vezérlőkkel hello kombinációja a feltételes hozzáférési házirend jelöli.

![vezérlő](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Vezérlők

A feltételes hozzáférési házirendek vezérlők meghatározása. azt, amely kell fordulhat elő, ha egy feltétel utasítás teljesítését.  
A vezérlők letiltja a hozzáférést, vagy a további követelményeknek megfelelő hozzáférést.
Amikor konfigurál egy házirendet, amely lehetővé teszi a hozzáférést, meg kell-e tooselect legalább egy követelmény.   

### <a name="grant-controls"></a>Támogatás vezérlők
hello aktuális végrehajtása az Azure Active Directory lehetővé teszi tooconfigure hello grant Alkalmazásvezérlési követelményeit a következő:

![vezérlő](./media/active-directory-conditional-access-azure-portal/05.png)

- **A multi-factor Authentication** -megkövetelheti az erős hitelesítés, a többtényezős hitelesítést. -Szolgáltatóként használhatja az Azure multi-factor Authentication vagy egy helyszíni többtényezős hitelesítési szolgáltató, Active Directory összevonási szolgáltatások (AD FS) együtt. A multi-factor authentication segítségével erőforrások védelme a fent konfigurált egy jogosulatlan felhasználó, aki előfordulhat, hogy rendelkezik kiszolgálószoftvertől toohello érvényes felhasználó hitelesítő adatait.

- **Megfelelő eszközökre** -hello eszköz szintjén is beállíthatja a feltételes hozzáférési szabályzatokat. Beállíthat egy házirendet tooonly engedélyezése számítógépek megfelelő, vagy a mobil eszközök, amelyek a mobileszköz-kezelési tooaccess-ban regisztrált a szervezet erőforrásaihoz. Például használja az Intune toocheck az eszköz megfelelőségét, és jelentést készít az tooAzure kényszerített AD amikor hello felhasználó próbál tooaccess egy alkalmazás. Hogyan toouse Intune tooprotect alkalmazásokat és adatokat, lásd: tartalmaznak részletes útmutatást [alkalmazások és a Microsoft Intune-nal adatainak védelme](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune). Intune tooenforce adatvédelem elveszett vagy ellopott eszközök is használja. További információkért lásd: [az adatok védelme teljes vagy szelektív törléssel a Microsoft Intune használatával](https://docs.microsoft.com/intune-classic/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

- **Tartományhoz csatlakozó eszközök** – hello eszköz tooconnect tooAzure Active Directory toobe tartományhoz tooyour használja a helyszíni Active Directory (AD) is megkövetelhető. Ez a házirend tooWindows asztali gépek, laptopok és vállalati táblagépeken vonatkozik. 

Ha több vezérlő kijelölt, is beállítható, hogy az összes lesz szükség, ha a házirend feldolgozása.

![vezérlő](./media/active-directory-conditional-access-azure-portal/06.png)

### <a name="session-controls"></a>Munkamenet-vezérlők
Munkamenet vezérlők a felhőalapú alkalmazások korlátozó tapasztalatok teszik lehetővé. hello munkamenet vezérlők felhőalkalmazások érvényesíti, és az Azure AD toohello alkalmazás hello munkamenetre vonatkozó kiegészítő információk támaszkodnak.

![vezérlő](./media/active-directory-conditional-access-azure-portal/session-control-pic.png)

#### <a name="use-app-enforced-restrictions"></a>Használja a kényszerített alkalmazásra vonatkozó korlátozások
A vezérlő toorequire az Azure AD toopass hello eszköz információk toohello cloud app is használhatja. Ezzel a megoldással hello felhő alkalmazás megállapítsa, ha hello felhasználó a megfelelő eszköz vagy a tartományhoz csatlakoztatott eszköz származik. Ez a vezérlő jelenleg csak a SharePoint hello felhő alkalmazásként támogatott. SharePoint egy korlátozott, és a teljes élmény használ hello eszköz információk tooprovide felhasználók hello eszköz állapotától függően.
toolearn hogyan toorequire korlátozott hozzáférés a SharePoint, kapcsolatos további információkért lépjen [Itt](https://aka.ms/spolimitedaccessdocs).

## <a name="condition-statement"></a>Feltételutasításhoz

hello előző szakaszban vezetett, toosupported beállítások tooblock, vagy korlátozhatja a vezérlők űrlapon tooyour erőforrások eléréséhez. A feltételes hozzáférési házirendek teljesül toobe szükséges a vezérlők toobe feltétel utasítás formában alkalmazott hello feltételeket adhat meg.  

Megadhat hello hozzárendelések a feltételutasításhoz be a következő:

![vezérlő](./media/active-directory-conditional-access-azure-portal/07.png)


- **Aki** -sok esetben érdemes a vezérlők alkalmazása toobe tooa a felhasználók adott csoportja. A feltétel utasításban definiálhat a kiválasztásával hello felhasználókat és csoportokat a házirend vonatkozik. Ha szükséges, explicit módon is kizárhat a felhasználók egy csoportja a házirend adásával.  
Felhasználók és csoportok kiválasztásával adja meg a házirend vonatkozik felhasználók hello hatókörét.    

    ![vezérlő](./media/active-directory-conditional-access-azure-portal/08.png)



- **Mi** -általában, vannak bizonyos alkalmazások, a védelem szempontjából, mint a többire további figyelmet igénylő környezetében futnak. Ez hatással van, például alkalmazásokat, amelyek hozzáférést toosensitive adatokat.
Felhőalkalmazások kiválasztásával a házirend vonatkozik felhőalkalmazások hello hatóköre határozza meg. Ha szükséges, akkor közvetlenül is kizárhatja. utóbbi esetben az alkalmazások a házirend.

    ![vezérlő](./media/active-directory-conditional-access-azure-portal/09.png)


- **Hogyan** - mindaddig, amíg hozzáférést tooyour alkalmazások körülmények szabályozhatja, mert előfordulhat, hogy nincs szükség további vezérlők előíró a hogyan a felhőalapú alkalmazások vannak elérhetők a felhasználók által történik. Dolog azonban eltérő, ha hozzáférést tooyour felhőalkalmazások hajtja végre, például a nem megbízható hálózatokhoz, vagy nem megfelelő eszközök lehet. A feltétel utasításban bizonyos hozzáférési feltételek, amelyek további követelményei: hogyan történik a hozzáférés tooyour alkalmazások adhat meg.

    ![Feltételek](./media/active-directory-conditional-access-azure-portal/21.png)


## <a name="conditions"></a>Feltételek

Hello aktuális végrehajtása az Azure Active Directory a következő területeken hello feltételei segítségével megadhatja:

- Bejelentkezési kockázata
- Eszközök
- Helyek
- Ügyfél-alkalmazások

![Feltételek](./media/active-directory-conditional-access-azure-portal/21.png)

### <a name="sign-in-risk"></a>Bejelentkezési kockázata

A bejelentkezési kockázata egy objektum által használt Azure Active Directory tootrack hello valószínűsége, hogy egy bejelentkezési kísérlet után nem lett végrehajtva hello jogos tulajdonos egy felhasználói fiók. Ez az objektum hello valószínűsége (magas, közepes vagy alacsony) képernyőn nevű attribútum tárolja [bejelentkezési kockázati szint](active-directory-reporting-risk-events.md#risk-level). Ez az objektum bejelentkezéskor a felhasználó generál, ha az Azure Active Directory bejelentkezési kockázatok észlelt. További részletek: [Kockázatos bejelentkezések](active-directory-identityprotection.md#risky-sign-ins).  
A feltételes hozzáférési házirendben feltételként számított hello bejelentkezési kockázati szint is használhatja. 

![Feltételek](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Eszközök

hello eszközplatform az eszközön futó operációs rendszer hello jellemzői a következők:

- Android
- iOS
- Windows Phone
- Windows
- macOS (előzetes verzió). 

![Feltételek](./media/active-directory-conditional-access-azure-portal/02.png)

Megadhatja, hogy hello eszközplatformokat, amelyeknek tartalmazza, továbbá az eszközplatformokat, amelyeken a házirend alól.  
toouse eszközplatformok hello házirendben, első módosítás hello konfigurálása váltógombok túl**Igen**, és válassza ki az összes, vagy egyéni eszköz platformok hello házirend vonatkozik. Ha az egyes eszközplatformok, hello házirend ezekről a platformokról csak hatással van. Ebben az esetben bejelentkezések tooother támogatott platformok nem hello házirend által érintett.


### <a name="locations"></a>Helyek

hello helyre, amelynél hello hello ügyfél IP-cím alapján tooconnect tooAzure Active Directory használja. Ez a feltétel igényli ismeri a toobe **helyek nevű** és **MFA megbízható IP-címek**.  

**Helyek nevű** az Azure Active Directoryban, amely lehetővé teszi a szervezetek megbízható toolabel IP-címtartományok szolgáltatása. A környezetben, használhatja a hello észlelése hello környezetben elnevezett helyét [kockázati események](active-directory-reporting-risk-events.md) és a feltételes hozzáférés. Az Azure Active Directory helyek nevű konfigurálásával kapcsolatos további részletekért lásd: [az Azure Active Directory helyek nevű](active-directory-named-locations.md).

Konfigurálható helyek hello számának hello kapcsolódó objektum hello mérete korlátozza az Azure ad-ben. Konfigurálhatja:
 
 - Egy olyan hellyel, amely másolatot too500 IP-címtartományok nevű
 - Legfeljebb 60 elnevezett helyek (előzetes verzió) egy IP-címtartománnyal rendelkező hozzárendelt azok tooeach 


**Többtényezős hitelesítés a megbízható IP-címek** , amely lehetővé teszi a szervezet helyi intranet képviselő toodefine megbízható IP-címtartományok multi-factor Authentication szolgáltatás. Egy hely feltételeinek konfigurálásakor a megbízható IP-címek lehetővé teszi toodistinguish a vállalati hálózathoz, és az egyéb helyekre kapcsolatok között. További részletekért lásd: [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  



Belefoglalhatja minden hely vagy a megbízható IP-címek és kizárhatja az összes megbízható IP-címek.

![Feltételek](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-app"></a>Ügyfélalkalmazás

hello ügyfélalkalmazás lehet (webböngésző, mobilalkalmazás, asztali ügyfél) általános szintű hello alkalmazások tooconnect tooAzure Active Directory használt vagy kifejezetten választhatja ki az Exchange Active Sync.  
Régebbi hitelesítési alapszintű hitelesítés, mint például a régebbi Office-ügyfelek, amelyek nem használják a modern hitelesítést használó tooclients hivatkozik. Feltételes hozzáférés jelenleg nem támogatott a hagyományos hitelesítéssel.

![Feltételek](./media/active-directory-conditional-access-azure-portal/04.png)


## <a name="common-scenarios"></a>Gyakori forgatókönyvek

### <a name="requiring-multi-factor-authentication-for-apps"></a>Többtényezős hitelesítés megkövetelése az alkalmazások

Sok környezetben a magasabb szintű védelem hello mint mások igénylő alkalmazások rendelkeznek.
Ez helyzet, például hello access toosensitive adatok rendelkező alkalmazások.
Ha azt szeretné, tooadd toothese alkalmazások védelmi réteget, konfigurálhatja egy feltételes hozzáférési szabályzatot, amely többtényezős hitelesítést igényel, amikor a felhasználók elérik-e ezeket az alkalmazásokat.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Többtényezős hitelesítés megkövetelése a nem megbízható hálózatokhoz való hozzáférést

Ebben a forgatókönyvben nem hasonló toohello előző helyzethez, mert a multi-factor authentication követelmény hozzáadása.
Hello fő különbség azonban ezt a követelményt hello feltételét.  
Hello célja az előző példában hello access toosensitve adatok az alkalmazások állapotában hello ebben a forgatókönyvben elsősorban megbízható helyeken.  
Ez azt jelenti lehetséges, hogy a multi-factor authentication követelmény az alkalmazások a felhasználó nem megbízható hálózaton keresztül illetéktelen.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz

Ha Intune használ a környezetben, azonnal megkezdheti hello feltételes hozzáférési házirendek felülete a hello Azure-konzolon.

Számos Intune használják az ügyfelek a feltételes hozzáférés tooensure, hogy csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz. Ez azt jelenti, hogy a mobileszközök az Intune-nal beléptetett és megfelelőségi házirend követelményeknek, és a Windows rendszerű számítógépek illesztett tooan helyszíni tartományban. A kulcs fokozása, hogy nem rendelkezik tooset ugyanabban a házirendben hello hello Office 365-szolgáltatások.  Ha egy új házirendet hoz létre, konfigurálja a hello Cloud apps tooinclude minden feltételes hozzáféréssel rendelkező tooprotect kívánja hello Office 365-alkalmazások.

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogyan tooconfigure egy feltételes hozzáférési szabályzatot: tooknow [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal-get-started.md).

Ha készen áll a tooconfigure feltételes hozzáférési házirendek a környezetéhez, lásd: hello [ajánlott eljárások a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-best-practices.md). 
