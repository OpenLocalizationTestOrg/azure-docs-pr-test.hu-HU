---
title: "Az Azure AD Connect: Áteresztő hitelesítés – hogyan működik? | Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Active Directory áteresztő hitelesítés működéséről."
services: active-directory
keywords: "Az Azure AD Connect áteresztő hitelesítés, telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: ffcebee572a9ba2840e81250651dea45599d65d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Az Azure Active Directory átmenő hitelesítést: Műszaki részletes bemutatója

>[!IMPORTANT]
>Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió. 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Hogyan működik az Azure Active Directory átmenő hitelesítést?

Következő lépések fordulhat elő, amikor a felhasználó toosign megpróbál az Azure Active Directory (Azure AD) által védett alkalmazáshoz, és ha átmenő hitelesítés engedélyezve van a hello bérlői hello:

1. hello felhasználó megpróbál tooaccess egy alkalmazás (például az Outlook Web App - hello https://outlook.office365.com/owa/).
2. Hello felhasználó már nem jelentkezett be, ha a hello felhasználó átirányított toohello az Azure AD bejelentkezési oldalára.
3. hello felhasználó felhasználónevét és jelszavát köt hello Azure AD bejelentkezési oldalára, és hello "Bejelentkezés" gombra kattint.
4. Az Azure Active Directory, a hello bejelentkezési kérelem fogadása hello felhasználónevét és jelszavát (nyilvános kulccsal titkosított) helyez egy üzenetsort.
5. A helyszíni áteresztő hitelesítés ügynök lehetővé teszi egy kimenő hívás toohello várólistát, és beolvassa a hello felhasználónévvel és jelszóval titkosított.
6. hello ügynök visszafejti hello jelszót a titkos kulccsal.
7. hello ügynök szerint hitelesíti hello felhasználónév és jelszó használatával normál Windows API-k (egy hasonló mechanizmus toowhat szolgál az Active Directory összevonási szolgáltatások) Active Directoryban. hello felhasználónév lehet vagy hello helyszíni alapértelmezett felhasználónév (általában `userPrincipalName`) vagy az Azure AD Connect konfigurált egy másik attribútum (úgynevezett `Alternate ID`).
8. hello a helyszíni Active Directory tartományvezérlőn (DC), majd hello kérelem kiértékeli, és értéket ad vissza megfelelő választ hello (sikeres, sikertelen, a jelszó lejárt, vagy felhasználói zárolása) toohello ügynök.
9. hello ügynök, pedig ez választ AD vissza tooAzure adja vissza.
10. Az Azure AD hello választ ad meg, és szükség szerint toohello felhasználói válaszol – például azt vagy hello felhasználó jelentkezik be azonnal, vagy kérelmeket a multi-factor Authentication (MFA).
11. Hello felhasználói bejelentkezés sikeres, hello felhasználói akkor tudja tooaccess hello alkalmazás.

hello alábbi ábrán látható összes hello összetevők és hello lépéseit.

![Átmenő hitelesítés](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Következő lépések
- [**Aktuális korlátozások** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – Ez a funkció jelenleg előzetes verzió. Ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.
- [**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -kérdések toofrequently válaszol.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
