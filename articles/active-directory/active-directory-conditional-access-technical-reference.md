---
title: "technikai útmutató az Active Directory feltételes hozzáférés aaaAzure |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi hello megadott feltételek hello felhasználói hitelesítés során, és mielőtt engedélyezi a hozzáférést toohello alkalmazás kiválasztása. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ee201405d1d17f130607a95bf455b60cd222dd0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Az Azure Active Directory feltételes hozzáférést biztosító műszaki útmutatója

## <a name="services-enabled-with-conditional-access"></a>Szolgáltatás engedélyezve van a feltételes hozzáféréssel

Feltételes hozzáférési szabályok több Azure AD-alkalmazás különféle is támogatott. A lista a következőket tartalmazza:


* Hello Azure Application Proxy regisztrált alkalmazások
* Azure távoli alkalmazás
* Fejlett üzletági és az Azure ad-vel regisztrált több-bérlős alkalmazásokhoz
* Dynamics CRM
* Összevont alkalmazásokhoz hello Azure AD alkalmazás-galériából
* A Microsoft Office 365 Yammer
* A Microsoft Office 365 Exchange online-hoz
* A Microsoft Office 365 SharePoint online-hoz (tartalmazza a onedrive vállalati verzió)
* Microsoft Power BI 
* Jelszó SSO alkalmazások hello Azure AD alkalmazás-galériából
* Visual Studio Team Services
* Microsoft Teams









## <a name="enable-access-rules"></a>Engedélyezze a hozzáférési szabályok
Minden egyes szabály is engedélyezhető vagy letiltható a egy alkalmazás körrel száma. Amikor a szabályok lettek **ON** azok engedélyezve lesz, és érvényesíti a hello alkalmazás elérő felhasználók számára. Ha vannak **OFF** nem lesz és fog nem gyakorolt hatás hello jelentkeznek élményt nyújt.

## <a name="applying-rules-toospecific-users"></a>Szabályok toospecific felhasználók alkalmazása
A szabályok lehetnek úgy, hogy a biztonsági csoport alapján alkalmazott toospecific felhasználócsoportokhoz **érvényes**. **Érvényes** túl beállítható**minden felhasználó** vagy **csoportok**. Ha értéke túl**minden felhasználó** hello szabályokat alkalmazza tooany felhasználói hozzáférés toohello alkalmazással. Hello **csoportok** beállítás lehetővé teszi, hogy bizonyos biztonsági és terjesztési csoportok toobe kijelölve, csak sikertelenek lehetnek ezekhez a csoportokhoz.

Szabály való telepítése esetén közös toofirst alkalmazhatja azt csak bizonyos felhasználók, a tesztelés csoportok tagjai. Miután a teljes hello szabály túl is alkalmazható**minden felhasználó**. Ennek hatására hello szabály toobe érvényesíti a hello szervezet összes felhasználója számára.

Csoportok is mentesíthetők hello használatával **kivéve** lehetőséget. Ezeket a csoportokat tagok fog kell sorolni, még akkor is, ha egy befoglalt csoportban szerepelnek.

## <a name="at-work-networks"></a>"Munkahelyi" hálózatok
Feltételes hozzáférési szabályok, amelyek egy "munkahelyi" hálózati támaszkodnak az Azure Active Directoryban beállított megbízható IP-címtartományok vagy a "belső vállalati hálózat" hello jogcímek az AD FS. A rendeletek az alábbiakat tartalmazzák:

* Ha nem munkahelyi többtényezős hitelesítést
* Letiltja a hozzáférést, ha nem a munkahelyi hálózatban

LDAP beállításai "munkahelyi" hálózatok

1. Konfigurálja a megbízható IP-címtartományok hello [többtényezős hitelesítés konfigurálása lapon](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Feltételes hozzáférési házirend minden egyes hitelesítési kérelmek és -tokennel kiállítási tooevaluate szabályok konfigurált hello címtartományokat fogja használni. 
2. Corpnet jogcím belül hello konfigurálhatja, ez a beállítás használható összevont könyvtárak, használja az AD FS szolgáltatást. toolearn belül corpnet jogcímek hello kapcsolatos további információkért lásd: [Tusted IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="rules-based-on-application-sensitivity"></a>Alkalmazás érzékenysége alapján
A szabályok konfigurálva vannak, így hello értékes szolgáltatások toobe tooother szolgáltatást befolyásolása nélkül védett alkalmazásonként. Feltételes hozzáférési szabályai lehet megadni a hello **konfigurálása** hello alkalmazás lapján. 

A szabályok jelenleg érhető el:

* **Többtényezős hitelesítés megkövetelése**
  
  * Minden felhasználó, hogy ez a házirend-e az alkalmazott toowill szükséges tooauthenticate legalább egyszer a többtényezős hitelesítéssel kell.
* **Ha nem munkahelyi többtényezős hitelesítést**
  
  * Ha a házirend érvényben van, minden felhasználó legalább egyszer végre szükséges toohave a multi-factor authentication lesz, ha munkaidőn kívüli távolról elérni azok hello szolgáltatást. Ha munkahelyi tooremote helyről helyezi át, fogják szükséges tooperform többtényezős hitelesítést, hello szolgáltatás elérésekor.
* **Letiltja a hozzáférést, ha nem a munkahelyi hálózatban** 
  
  * Ha a felhasználók munkahelyi tooa távoli helyről, le lesznek tiltva Ha hello "Letiltja a hozzáférést, ha nem munkahelyi" házirend alkalmazott toothem.  Ezek újra kapnak hozzáférést, a munkahelyi helyre.

## <a name="related-topics"></a>Kapcsolódó témakörök
* [Az Active Directory tooAzure tooOffice 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele csatlakoztatva](active-directory-conditional-access.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

