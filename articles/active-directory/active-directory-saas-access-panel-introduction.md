---
title: "aaaWhat hello hozzáférési panel az Azure Active Directoryban? | Microsoft Docs"
description: "Ismerje meg, hogyan férhetnek hozzá a hello toouse változatait (webböngésző, Android-alkalmazás, iPhone és iPad) panelen tooaccess SaaS-alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 800be6a69f13978c5b88e2fe28a416d4b763656c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-access-panel"></a>Mi az a hozzáférési panel hello?

hello hozzáférési panel egy webes portál. Ez lehetővé teszi, hogy a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory tooview és kezdő felhőalapú alkalmazásokban az Azure AD-rendszergazda engedélyezte őket a hozzáférést. Önkiszolgáló csoportkezelési és kezelési képességeit hello hozzáférési panel keresztül is használható.

hello hozzáférési panel elkülönül hello Azure-portálon, és does nem Ön toohave Azure-előfizetéssel.

![Hozzáférési panel][1]

hello hozzáférési panel lehetővé teszi a profil beállításait, beleértve néhány hello lehessen tooedit:

- A munkahelyi vagy iskolai fiókhoz kapcsolódó hello jelszó módosítása

- Jelszó alaphelyzetbe állítása beállításainak szerkesztése

- Ügyfél és a beállításokat szabályozó beállítások kapcsolódó toomulti tényezős hitelesítés szerkesztése (lett volna szükség toouse-fiókok, egy rendszergazda)

- A fiók adatait, például a felhasználói Azonosítóját, másodlagos e-mail, és a mobil- és office telefonszámokat, és az eszközök megtekintése

- Nézet és kezdő felhőalapú alkalmazások, amelyek az Azure AD rendszergazdai hello rendelkezik őket hozzáférést kap. Hello hozzáférési panel a hello felhasználók szempontjából kapcsolatos további információkért lásd: hello hozzáférési panel használatával. 

- Önálló csoportok kezelése. Pontosabban hello rendszergazda létrehozhat és biztonsági csoportok és a kérelem biztonsági csoporttagságok kezelése az Azure ad-ben. További információkért lásd: [önkiszolgáló csoportkezelési a felhasználók számára az Azure AD](active-directory-accessmanagement-self-service-group-management.md) és [saját csoportjai kezelését](active-directory-manage-groups.md).




## <a name="accessing-hello-access-panel"></a>Hello hozzáférési panel elérése

Hello hozzáférési panel érhetők el a következő URL-címet egy webböngészőben hello érhető el:`http://myapps.microsoft.com`

Ha a bejelentkezési lapon konfigurált egyéni védjegyek, betöltheti branding a szervezet tartomány toohello hello URL-cím végéhez hozzáfűzésével:`http://myapps.microsoft.com/<your domain>.com`

Ebben az esetben használhatja ki bármely aktív vagy ellenőrzött és érvényes tartománynevet, amely az Azure-portálon lett konfigurálva.

![A Wingtip Toys tartománynév][2]  

Meg kell toodistribute hello URL-cím tooall felhasználók, akik az Azure AD integrált tooapplications alá fogja írni.

## <a name="authentication"></a>Authentication

tooreach hello hozzáférési panelre, akkor kell hitelesíteni keresztül a munkahelyi vagy iskolai fiókkal az Azure ad-ben. Hitelesített tooAzure AD közvetlenül lehet. Azt is megteheti Ha egy szervezet összevonási konfigurált Active Directory összevonási szolgáltatások (AD FS) vagy egyéb technológiák használatával, akkor hitelesítheti a Windows Server Active Directory.

Az Azure vagy Office 365-előfizetéssel rendelkezik, és hello Azure-portálon vagy az Office 365 alkalmazást használja, ha hello alkalmazások listáját láthatja nélkül aláíró újra be. Ha Ön nem hitelesített használatával hello felhasználónevet és jelszót a fiókhoz az Azure ad-ben a kért toosign áll. Ha a szervezet összevonási konfigurált, írja be a hello felhasználónév is használhatók.

Megtörténik, amikor a rendszergazda hello Directoryval integrált alkalmazások hello kezelheti. Hogyan toointegrate alkalmazások az Azure AD: toolearn [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Webböngészőkre vonatkozó követelmények

Legalább a hello hozzáférési panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte. Hello a jelszó-alapú egyszeri bejelentkezést (SSO) keresztül tooapplications bejelentkezett felhasználó toobe hello hozzáférési panel bővítményét kell telepíteni a böngészőben. hello bővítmény le automatikusan, amikor kiválaszt egy alkalmazást, amely jelszóalapú SSO van konfigurálva.

hello hozzáférési panel bővítmény érhető el jelenleg Internet Explorer 8 és újabb verziók, Firefox, biztonsági és Chrome böngésző.

## <a name="mobile-app-support"></a>Mobilalkalmazás-támogatás

hello Azure Active Directory csapat hello tesz közzé alkalmazásokat a mobilalkalmazás. Hello alkalmazás telepítésekor bejelentkezhet toopassword-alapú egyszeri Bejelentkezést alkalmazások iOS és Android-eszközökön.

> [!NOTE]
> Bejelentkezhet az Azure ad-val (például Salesforce, Google Apps, Dropbox, mezőben, Concur, Workday, Office 365, és több mint 70 mások) összevonási támogató tooapplications szinte bármilyen böngészőben, bármilyen eszközön, anélkül, hogy a beépülő modul vagy mobil alkalmazások. Minden más [hozzáférési panel lép](https://myapps.microsoft.com/) is nem igényelnek az alkalmazások mobilalkalmazás toobe használni a mobileszköz hello.
>
>

### <a name="my-apps-for-android"></a>A saját Android-alkalmazások

A saját Android-alkalmazások számítógépre Android 4.1 Android-verziót futtat és újabb rendszer.  
Hello elérhető [Google Play áruház](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![A saját Android-alkalmazások][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>IPhone és iPad alkalmazásaimat

Az iOS-alkalmazások bármely iPhone vagy iPad futtató verziója iOS 7 és újabb verziók támogatják.  
Hello elérhető [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Az iOS-alkalmazások][4]    



## <a name="managed-browser-for-my-apps"></a>Felügyelt böngésző saját alkalmazások

Az alkalmazások is integrálva van a hello Intune felügyelt böngészővel. hello Intune Managed Browser iOS és Android-eszközök győződjön meg arról, hogy a mobileszközök adatainak biztonságos marad kulcsfontosságú szerepet játszik. Lehetővé teszi a biztonságosan megtekintheti, és keresse meg, amelyek a vállalati adatokat tartalmazhatnak, és biztonságos webböngésző élményt nyújt.  
A gyors hozzáférés toomy alkalmazásokat a Managed Browser kezdőlap és a könyvjelzőt, így kevesebb kattint tooreach bármely alkalmazást tooaccess megkeresése

Hello elérhető [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) és [Google Play áruház](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Saját alkalmazások Mananged böngésző][5]    





## <a name="tips-for-testing-hello-user-experience"></a>Tippek a tesztelési hello felhasználói élmény

Ha egy Azure rendszergazdát, és be van jelentkezve toohello Azure-portálon hello directory egy olyan fiókkal, automatikusan bejelentkeztetjük toohello hozzáférési panelen, a jelenlegi fiókkal. Ebben az esetben tooyou hozzárendelt összes alkalmazás látható.

**mint tootest egy *különböző* felhasználói fiók:**

1. Hello jobb felső sarkában hello Azure-portálon vagy a hozzáférési panel hello hello felhasználói menüre kattint, és válassza **Kijelentkezés**. 
2. Nyissa meg toohello [hozzáférési panel](http://myapps.microsoft.com).
3. Hello bejelentkezési oldalára, típus hello felhasználónév és a címtárban hello fiók jelszavát kívánt tootest.


## <a name="starting-applications"></a>Alkalmazások elindítása

Számos különböző típusú alkalmazások hello hozzáférési panel megjelenhet.

### <a name="office-365-applications"></a>Office 365-alkalmazások

Ha a szervezet által használt Office 365-alkalmazások és a számukra rendelkezik licenccel, hello Office 365-alkalmazások jelennek meg a hozzáférési panel.

Ha egy alkalmazás csempe az Office 365 alkalmazás gombra kattint, alkalmazás és az automatikus bejelentkezés átirányított toohello áll.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások

A rendszergazda adhat hozzá alkalmazásokat hello hello Azure-portálon az Active Directory szakasza hello SSO módban a túl**az Azure AD az egyszeri bejelentkezés**. Csak megtekintheti ezeket az alkalmazásokat, ha a rendszergazda explicit módon adott hozzáférhessenek az toohello alkalmazásokhoz.

Ezen alkalmazások egyikét mozaikokra kattintva Ön átirányítva, és automatikusan megtörténik a toohello alkalmazás.

### <a name="password-based-sso-without-identity-provisioning"></a>Egyszeri bejelentkezés jelszó alapú identitás kiépítés nélkül

A rendszergazda adhat hozzá alkalmazásokat hello hello Azure-portálon az Active Directory szakasza hello SSO módban a túl**jelszó-alapú egyszeri bejelentkezést**. Hello címtárban szereplő összes felhasználó láthatja az összes olyan alkalmazások, amelyek ebben a módban vannak konfigurálva.

hello először, akkor egy csempére kattint, az alábbi alkalmazások közül, felszólító tooinstall hello jelszó SSO beépülő modul az Internet Explorer vagy a Chrome áll. hello telepítés akkor toorestart lehet szükség a webböngésző. Ha toohello hozzáférési panel vissza, és kattintson hello alkalmazás csempéjére újra, a felhasználónév és jelszó hello alkalmazás kéri. Miután megadta a felhasználónevét és jelszavát, ezeket a hitelesítő adatokat biztonságosan tárolja, és tooyour fiók társított Azure AD-ben.

hello kattintson hello alkalmazás csempéjére, automatikusan bejelentkezik az toohello alkalmazás következő indításakor.  
Nem rendelkezik tooenter a hitelesítő adatokkal megismételni és vagy hello jelszó SSO beépülő modul telepítése.

Ha a hitelesítő adatok hello külső célalkalmazásban módosult, frissíteni kell az Azure AD-ben tárolt hitelesítő adatokat. 

**tooupdate hitelesítő adatait:**

1. Válassza ki a hello alkalmazás csempére hello ikon.
2. Válassza ki **hitelesítő adatainak frissítése** tooreenter hello felhasználónevét és jelszavát hello alkalmazás.


### <a name="password-based-sso-with-identity-provisioning"></a>Jelszó-alapú egyszeri bejelentkezés identitás kiépítése

A rendszergazda adhat hozzá alkalmazásokat hello **Active Directory** hello Azure-portálon hello SSO módba állítja túl**jelszó-alapú egyszeri bejelentkezést**, identitás-kiépítés együtt.

hello első alkalommal kattint az alkalmazás csempéjére a hozzá tartozó egyik ezeket az alkalmazásokat, rákérdezéses tooinstall hello **jelszó SSO beépülő modul az Internet Explorer vagy a Chrome**. hello telepítés akkor toorestart lehet szükség a webböngésző.  
Ha toohello hozzáférési panel vissza, és gombra hello alkalmazás csempe, automatikusan bejelentkeztetjük toohello alkalmazásban.

Egyes alkalmazások előfordulhat, hogy Ön toochange a jelszó kérése a hello első bejelentkezés. Ha a hitelesítő adatok hello külső célalkalmazásban módosult, hello Azure AD-ben tárolt hitelesítő adatok is frissítenie kell. 

**tooupdate hitelesítő adatait:**

1. Válassza ki a hello alkalmazás csempére hello ikon.
2. Válassza ki **hitelesítő adatainak frissítése** tooreenter hello felhasználónevét és jelszavát hello alkalmazás.


### <a name="application-with-existing-sso-solutions"></a>A már meglévő SSO megoldásaival alkalmazás

tooconfigure egyszeri bejelentkezés egy alkalmazás, hello Azure-portálon biztosít a harmadik lehetőség nevű **meglévő egyszeri bejelentkezés**. Ez a beállítás lehetővé teszi, hogy a rendszergazda toocreate egy kapcsolat tooan alkalmazás, és helyezze el a kijelölt felhasználók hello hozzáférési panel.

Például ha egy alkalmazás konfigurált tooauthenticate felhasználók az AD FS 2.0 használatával, a rendszergazda használható hello **meglévő egyszeri bejelentkezés** beállítás toocreate egy hivatkozás tooit hello hozzáférési panelen. Hello hivatkozás fér hozzá, amikor a rendszer hitelesíti az AD FS 2.0 vagy bármilyen meglévő SSO megoldás hello alkalmazás maga biztosítja.


## <a name="next-steps"></a>Következő lépések

- toosee kapcsolódó tooapplication kezelése, amelyek témakörök listáját lásd: hello [cikk index az Azure Active Directoryban Alkalmazáskezelés](active-directory-apps-index.md).
 
- toolearn hogyan toointegrate egy SaaS-alkalmazás az Azure AD-be: hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazások](active-directory-saas-tutorial-list.md).
 
- További információ az alkalmazások kezelése az Azure AD toolearn lásd: hello [bemutatása toosingle bejelentkezés és az Azure Active Directoryval való hozzáférésének kezelése](active-directory-appssoaccess-whatis.md).
 
- toolearn a felhasználók átadása, kapcsolatos további információkért lásd: [felhasználói kiépítésének és megszüntetésének biztosítása tooSaaS alkalmazások automatizálása](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
