---
title: "Az Azure Active Directory feltételes hozzáférést biztosító műszaki útmutatója |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést az Azure Active Directory ellenőrzi a megadott feltételek, ha a felhasználó hitelesítése és az alkalmazáshoz való hozzáférés előtt válasszon. Ha ezek a feltételek teljesülnek, a felhasználó hitelesítése és hozzáférni az alkalmazáshoz engedélyezett."
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
ms.openlocfilehash: ca16a5399f94fd1ab267e0798cade3fd83f75b13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Az Azure Active Directory feltételes hozzáférést biztosító műszaki útmutatója

## <a name="services-enabled-with-conditional-access"></a>Szolgáltatás engedélyezve van a feltételes hozzáféréssel

Feltételes hozzáférési szabályok több Azure AD-alkalmazás különféle is támogatott. A lista a következőket tartalmazza:


* Az Azure alkalmazásproxyval regisztrált alkalmazások
* Azure távoli alkalmazás
* Fejlett üzletági és az Azure ad-vel regisztrált több-bérlős alkalmazásokhoz
* Dynamics CRM
* Összevont alkalmazásokhoz az Azure AD alkalmazás-galériából
* A Microsoft Office 365 Yammer
* A Microsoft Office 365 Exchange online-hoz
* A Microsoft Office 365 SharePoint online-hoz (tartalmazza a onedrive vállalati verzió)
* Microsoft Power BI 
* Jelszó SSO alkalmazások az Azure AD alkalmazás-galériából
* Visual Studio Team Services
* Microsoft Teams









## <a name="enable-access-rules"></a>Engedélyezze a hozzáférési szabályok
Minden egyes szabály is engedélyezhető vagy letiltható a egy alkalmazás körrel száma. Amikor a szabályok lettek **ON** azok engedélyezve lesz, és a felhasználók számára az alkalmazás elérésének érvényesíti. Ha vannak **OFF** nem fogja használni, és nem befolyásolja a felhasználók bejelentkezési élményt nyújt.

## <a name="applying-rules-to-specific-users"></a>Szabályok alkalmazása adott felhasználókra
Szabályok alkalmazhatók a biztonsági csoport alapján úgy, hogy a felhasználók adott részhalmazához **érvényes**. **Érvényes** megadható **minden felhasználó** vagy **csoportok**. Ha beállítása **minden felhasználó** szabályok minden olyan felhasználó, az alkalmazás települ. A **csoportok** beállítással meghatározott biztonsági és terjesztési csoportot ki kell választani, csak sikertelenek lehetnek ezekhez a csoportokhoz.

Egy szabály telepítésekor esetében gyakori, először alkalmazni a korlátozott állítva, a felhasználók, amelyek a tesztelés csoportok tagjai. Miután befejeződött a szabály is alkalmazható **minden felhasználó**. Ennek hatására a szabály, amely érvényesíti a szervezet összes felhasználója számára.

Csoportok is mentesíthetők házirendekkel a **kivéve** lehetőséget. Ezeket a csoportokat tagok fog kell sorolni, még akkor is, ha egy befoglalt csoportban szerepelnek.

## <a name="at-work-networks"></a>"Munkahelyi" hálózatok
Feltételes hozzáférési szabályok, amelyek egy "munkahelyi" hálózati támaszkodnak az Azure Active Directoryban beállított megbízható IP-címtartományok vagy a "belső vállalati hálózat" jogcímek az AD FS használatára. A rendeletek az alábbiakat tartalmazzák:

* Ha nem munkahelyi többtényezős hitelesítést
* Letiltja a hozzáférést, ha nem a munkahelyi hálózatban

LDAP beállításai "munkahelyi" hálózatok

1. Konfigurálja a megbízható IP-címtartományok a [többtényezős hitelesítés konfigurálása lapon](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Feltételes hozzáférési házirend használatával fogja a beállított tartomány minden egyes hitelesítési kérelmek és a hitelesítési karakterlánc-kiállítási szabályok kiértékelése. 
2. Konfigurálhatja a belső vállalati hálózat jogcím, ez a beállítás használható összevont könyvtárak, használja az AD FS szolgáltatást. További információ a belső vállalati hálózat jogcímeket, lásd: [Tusted IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="rules-based-on-application-sensitivity"></a>Alkalmazás érzékenysége alapján
A szabályok konfigurálva vannak alkalmazásonként engedélyezése a nagy értékű szolgáltatásokat érintő egyéb szolgáltatásokhoz való hozzáférés nélkül védeni. Feltételes hozzáférési szabályai konfigurálhatja a **konfigurálása** az alkalmazás lapján. 

A szabályok jelenleg érhető el:

* **Többtényezős hitelesítés megkövetelése**
  
  * Ez a házirend vonatkozik az összes felhasználó legalább egyszer a többtényezős hitelesítéssel hitelesíteni kell.
* **Ha nem munkahelyi többtényezős hitelesítést**
  
  * Ha a házirend érvényben van, minden felhasználó lesz szükség legalább egy alkalommal a multi-factor authentication végeztek, ha azok munkaidőn kívüli távolról elérni a szolgáltatást. Ha azok áthelyezése egy távoli helyre, akkor le kell többtényezős hitelesítést végezni, ha a szolgáltatás elérésével.
* **Letiltja a hozzáférést, ha nem a munkahelyi hálózatban** 
  
  * Ha a felhasználók a munkahelyen egy távoli helyre, le lesznek tiltva Ha a "Letiltja a hozzáférést, ha nem munkahelyi" házirend alkalmazását.  Ezek újra kapnak hozzáférést, a munkahelyi helyre.

## <a name="related-topics"></a>Kapcsolódó témakörök
* [Az Azure Active Directoryhoz csatlakoztatott Office 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele](active-directory-conditional-access.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

