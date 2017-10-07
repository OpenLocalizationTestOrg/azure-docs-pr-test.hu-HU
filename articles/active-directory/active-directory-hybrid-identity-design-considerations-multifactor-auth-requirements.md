---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - többtényezős hitelesítési követelmények meghatározása"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi hello megadott feltételek hello felhasználói hitelesítés során, és mielőtt engedélyezi a hozzáférést toohello alkalmazás kiválasztása. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 49fa7b43772fb3a2d6664747477c60a34cddde2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitáskezelési megoldás a többtényezős hitelesítési követelmények meghatározása
A világ mobilitási, a felhasználói adatok és alkalmazások hello felhőben, és egy eszközről ezek az információk védelme vált kiemelkedő.  Minden nap van egy új főcím kapcsolatos biztonsági problémák.  Bár nem garantálja az ilyen problémák elleni van, a többtényezős hitelesítés további réteget biztosít biztonsági toohelp megakadályozzák e problémák.
Indítsa el a multi-factor authentication hello szervezetek szükséges követelmények értékelésekor. Ez azt jelenti, hogy mi az hello szervezet közben toosecure.  Ez a kiértékelés fontos toodefine hello műszaki követelményeiben és hello szervezetek felhasználók a multi-factor authentication.

> [!NOTE]
> Ha nem ismeri a többtényezős hitelesítés és a hatása, erősen ajánlott, hogy olvassa el a hello cikk [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) előzetes toocontinue a fejezet elolvasása.
> 
> 

Győződjön meg arról, hogy tooanswer hello következő:

* A vállalat próbál toosecure Microsoft-alkalmazások? 
* Hogyan közzétett ezeket az alkalmazásokat?
* A vállalat biztosítja a távelérés tooallow alkalmazottak tooaccess a helyszíni alkalmazások?

Ha igen, milyen típusú távelérési? Szükség tooevaluate, amelyben hello elérő felhasználók számára az alkalmazások található. Az értékelés egy másik fontos lépés toodefine hello megfelelő a multi-factor authentication stratégia. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Hol található a hello felhasználók toobe is?
* Ezek lehetnek bárhol?
* Nem a vállalat szeretne tooestablish korlátozások toohello felhasználó földrajzi helye szerint?

Ezek a követelmények elsajátítása után fontos tooalso értékelje ki a multi-factor authentication hello felhasználói követelmények. Ez a kiértékelés fontos, mert azt határozza meg a multi-factor authentication terítésével hello követelményei. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Jártas a hello felhasználók a többtényezős hitelesítés?
* Néhány felhasználását lesz szükség tooprovide további hitelesítési?  
  * Ha igen, az összes hello idő, a külső hálózatokat vagy férnek hozzá bizonyos alkalmazásokat, vagy más feltételek mellett?
* Hello felhasználóinak kell hogyan képzési toosetup és alkalmazzon a multi-factor authentication?
* Mik azok a hello főbb forgatókönyvek, hogy a vállalat szeretne-e a felhasználóik tooenable többtényezős hitelesítést?

Hello előző kérdések megválaszolásával után nem tud toounderstand fogja már megvalósította a multi-factor authentication helyszíni esetén. Ez a kiértékelés fontos toodefine hello műszaki követelményeiben és hello szervezetek felhasználók a multi-factor authentication. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* A vállalatának meg kell tooprotect rendszerjogosultságú fiókok az MFA Használatát?
* A vállalatának meg kell tooenable MFA egyes alkalmazás megfelelőségi okokból?
* A vállalatának meg kell tooenable MFA ezen alkalmazás-vagy csak a rendszergazdák az összes jogosult felhasználók számára?
* Szükség van mindig engedélyezve van az MFA- vagy csak amikor hello felhasználók bejelentkeznek a vállalati hálózaton kívül?

## <a name="next-steps"></a>Következő lépések
[A hibrid identitás bevezetési stratégia meghatározása](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

