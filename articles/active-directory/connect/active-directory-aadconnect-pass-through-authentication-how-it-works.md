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
ms.openlocfilehash: d34ccd40082edbe036d963ad548bff648119bdd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Az Azure Active Directory átmenő hitelesítést: Műszaki részletes bemutatója

>[!IMPORTANT]
>Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió. 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Hogyan működik az Azure Active Directory átmenő hitelesítést?

Amikor egy felhasználó próbál bejelentkezni az Azure Active Directory (Azure AD) által védett alkalmazáshoz, és az áteresztő hitelesítés engedélyezve van a bérlő, a következő lépések következnek:

1. A felhasználó megpróbál hozzáférni az alkalmazáshoz (például az Outlook Web App - https://outlook.office365.com/owa/).
2. Ha a felhasználó nem jelentkezett, a felhasználót a rendszer átirányítja az Azure AD bejelentkezési oldalára.
3. A felhasználó felhasználónevét és jelszavát köt az Azure AD bejelentkezési oldal, és a "Bejelentkezés" gombra kattint.
4. Az Azure Active Directory, a bejelentkezési kérelem fogadása helyez el a felhasználónevet és jelszót (a nyilvános kulccsal titkosított) várólista.
5. Egy helyszíni áteresztő hitelesítési ügynök a várólistára kimenő hívást, és lekéri a felhasználónév és a titkosított jelszót.
6. Az ügynök visszafejti a jelszót a titkos kulccsal.
7. Az ügynök szerint hitelesíti a felhasználónevet és jelszót normál Windows API-k (hasonló eljárást, amely Active Directory összevonási szolgáltatások által használttól) Active Directoryban. A felhasználónév vagy a helyszíni alapértelmezett felhasználónév lehet (általában `userPrincipalName`) vagy az Azure AD Connect konfigurált egy másik attribútum (úgynevezett `Alternate ID`).
8. A helyszíni Active Directory tartományvezérlőn (DC) ezután kiértékeli a kérelmet, és a megfelelő választ ad vissza (sikeres, sikertelen, a jelszó lejárt, vagy felhasználói zárolása) ügynökkel.
9. Az ügynök visszaadó ezt a választ az Azure AD vissza.
10. Az Azure AD a válasz kiértékeli, és válaszol-e a felhasználó megfelelő – például azt vagy a felhasználó jelentkezik be azonnal, vagy kérelmeket a multi-factor Authentication (MFA).
11. Ha a felhasználói bejelentkezés sikeres, a felhasználó nem tud hozzáférni az alkalmazáshoz.

A következő ábra összetevőit és a lépéseit mutatja be.

![Áteresztő hitelesítés](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Következő lépések
- [**Aktuális korlátozások** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – Ez a funkció jelenleg előzetes verzió. Ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.
- [**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -gyakran feltett kérdésekre adott válaszokat.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -Útmutató: a szolgáltatással kapcsolatos gyakori problémák megoldása.
- [**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
