---
title: "feltételes hozzáférés az Azure Active Directoryban aaaBest eljárásai |} Microsoft Docs"
description: "További tudnivalók a dolgot tudnia kell, és mi ezzel kerülendő a feltételes hozzáférési szabályzatok konfigurálásakor."
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
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4952f8746a2e583380b3bb99cfe2fbdae1c07b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Gyakorlati tanácsok a feltételes hozzáférés az Azure Active Directoryban

Ez a témakör a dolgot tudnia kell, és mi ezzel kerülendő a feltételes hozzáférési szabályzatok konfigurálásakor kapcsolatos információkat nyújt. Ebben a témakörben olvasásakor, előtt tanulmányozza át kapcsolatos hello fogalmakat és hello terminológia leírt [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Tudnivalók

### <a name="whats-required-toomake-a-policy-work"></a>Mi szükséges toomake házirend munkahelyi?

Ha egy új házirendet hoz létre, nincsenek felhasználók, csoportok, alkalmazások vagy kiválasztott hozzáférés-vezérlést.

![Felhőalkalmazások](./media/active-directory-conditional-access-best-practices/02.png)


toomake a házirend működik, konfigurálnia kell a következő hello:


|mi           | Hogyan                                  | miért|
|:--            | :--                                  | :-- |
|**Felhőalkalmazások** |Tooselect kell egy vagy több alkalmazást.  | hello feltételes hozzáférési házirend célja tooenable meg toofine-hangolási hogyan engedéllyel rendelkező felhasználók férhetnek hozzá az alkalmazásokat.|
| **Felhasználók és csoportok** | Szüksége tooselect legalább egy felhasználót vagy csoportot, amely jogosult a kiválasztott tooaccess hello felhőalapú alkalmazásokat. | A feltételes hozzáférési szabályzat, amelynek nincs hozzárendelt felhasználók és csoportok, soha nem aktiválódott. |
| **Hozzáférés-vezérlést** | Legalább egy tooselect kell hozzáférés-vezérlést. | A házirend processzor kell tooknow milyen toodo a feltételek teljesülése esetén.|


Továbbá toothese alapvető követelményeivel, sok esetben a is konfigurálnia kell egy feltételt. Egy házirend akkor is megvalósítható, ha egy konfigurált feltételt, feltételek hello vezetői tényező a pontosabb beállításra hozzáférés tooyour alkalmazásokat is.


![Felhőalkalmazások](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Hogyan hozzárendelések értékeli ki a rendszer?

Minden hozzárendeléseket a rendszer logikailag **műveletet**. Ha egynél több hozzárendelés konfigurálva, egy házirend tootrigger vonatkozó összes hozzárendelést kell biztosítani.  

Ha módosítania kell a vállalati hálózaton kívülről érkezett tooall kapcsolatok olyan hely feltételt tooconfigure, ez által elvégezhető:

- Beleértve **az összes hely**
- Kivéve **a megbízható IP-címek**

### <a name="what-happens-if-you-have-policies-in-hello-azure-classic-portal-and-azure-portal-configured"></a>Mi történik, ha a klasszikus Azure portálon hello házirendek és konfigurált Azure-portálon?  
Mindkét házirend Azure Active Directory érvényesíti, és hello felhasználó élvezheti a hozzáférést, csak akkor, ha összes követelmény teljesülését.

### <a name="what-happens-if-you-have-policies-in-hello-intune-silverlight-portal-and-hello-azure-portal"></a>Mi történik, ha a házirendek segítségével a hello Intune Silverlight portal és hello Azure-portálon?
Mindkét házirend Azure Active Directory érvényesíti, és hello felhasználó élvezheti a hozzáférést, csak akkor, ha összes követelmény teljesülését.

### <a name="what-happens-if-i-have-multiple-policies-for-hello-same-user-configured"></a>Mi történik, ha ugyanazon felhasználó konfigurált hello több házirendeket?  
Minden bejelentkezés az Azure Active Directory kiértékeli az összes házirend, és biztosítja, hogy minden követelmények teljesülnek-e megadott hozzáférési toohello felhasználói előtt.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Feltételes hozzáférés az Exchange ActiveSync szolgáltatással működik?

Igen, az Exchange ActiveSync is használhatja a feltételes hozzáférési házirendben.


## <a name="what-you-should-avoid-doing"></a>Mi kerülendő ezzel

hello feltételes hozzáférés keretrendszer egy nagyszerű konfigurációs rugalmasságot biztosít. Azonban rugalmas lehetőségeket biztosítanak azt is jelenti, hogy alaposan tekintse át minden konfigurációs házirend előzetes tooreleasing azt tooavoid nemkívánatos eredmények. Ebben a környezetben, mint a teljes érintő különös figyelmet tooassignments kiválasztásánál kell **minden felhasználók / csoportok / felhőalapú alkalmazásokba**.

A környezetben a következő konfigurációk hello kerülje el:


**Az összes felhasználó számára az összes felhőalapú alkalmazásokat:**

- **Letiltja a hozzáférést,** -ebben a konfigurációban a teljes szervezet, amely nincs mindenképpen célszerű blokkolja.

- **Megfelelő eszközökre szükséges** – a felhasználók számára, amely nem regisztrálták az eszközeiket, mégis a házirend nem engedélyezi az összes eléréséhez, beleértve a hozzáférési toohello Intune portál. Ha Ön rendszergazda a regisztrált eszköz nélküli, akkor ez a házirend letiltja a hello Azure portál toochange hello házirendbe vissza fog.

- **Megkövetelje a tartományhoz való csatlakozást** – Ez a házirend-blokk hozzáférést is hello lehetséges tooblock hozzáféréssel rendelkezik az összes felhasználó számára a szervezet Ha még nem rendelkezik a tartományhoz csatlakoztatott eszközön.


**Az összes felhasználó számára az összes felhőalapú alkalmazások, az összes eszközplatformra:**

- **Letiltja a hozzáférést,** -ebben a konfigurációban a teljes szervezet, amely nincs mindenképpen célszerű blokkolja.


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
