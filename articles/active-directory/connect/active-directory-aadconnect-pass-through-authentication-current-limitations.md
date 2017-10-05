---
title: "Az Azure AD Connect: Áteresztő hitelesítés – aktuális korlátozásai |} Microsoft Docs"
description: "Ez a cikk ismerteti a jelenlegi korlátozások az Azure Active Directory (Azure AD) áteresztő hitelesítés."
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
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 37c0ea094d02208f2516a4a040f75894e046c670
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Az Azure Active Directory átmenő hitelesítést: Aktuális korlátozások

>[!IMPORTANT]
>Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió. Egy szabad szolgáltatást, és nem kell használni az Azure AD bármely fizetős verziója. Áteresztő hitelesítés csak érhető el a világméretű példányát az Azure AD, és nem a [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) és [Microsoft Azure Government felhő](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Támogatott helyzetek

Előzetes teljes mértékben támogatja a következő esetekben:

- Felhasználói bejelentkezések minden böngésző alapú webalkalmazás.
- Felhasználói bejelentkezések be, amely támogatja az Office 365-ügyfélalkalmazások [modern hitelesítést](https://aka.ms/modernauthga).
- Az Azure AD Join Windows 10 rendszerű eszközökhöz.
- Exchange ActiveSync-támogatását.

## <a name="unsupported-scenarios"></a>Nem támogatott forgatókönyvek

A következő forgatókönyvek _nem_ előzetes támogatott:

- Felhasználói bejelentkezések alkalmazásokba örökölt Office ügyfél (az Office 2013 vagy korábbi). A szervezetek javasolt, hogy váltani a modern hitelesítést, ha lehetséges. A modern hitelesítés lehetővé teszi az áteresztő hitelesítés támogatásához azonban is segítséget nyújt a felhasználó mindig lássa fiókok [feltételes hozzáférés](../active-directory-conditional-access.md) funkciók, például a többtényezős hitelesítés (MFA).
- Felhasználói bejelentkezések Skype az üzleti ügyfélalkalmazások, beleértve a Skype vállalati 2016 esetében.
- Felhasználói bejelentkezések a PowerShell 1.0-s verziója. Javasoljuk, hogy inkább PowerShell 2.0-s verzió.

>[!IMPORTANT]
>Nem támogatott forgatókönyvek megoldás, engedélyezze a Jelszókivonat-szinkronizálást a [választható szolgáltatások](active-directory-aadconnect-get-started-custom.md#optional-features) az Azure AD Connect varázsló lapján. Jelszókivonat-szinkronizálást úgy működik, mint az előző helyzetekben fallback _csak_ (és _nem_ általános tartalékként átmenő hitelesítés). Ha ezek a forgatókönyvek nem szükséges, kapcsolja ki a Jelszókivonat-szinkronizálást.

## <a name="next-steps"></a>Következő lépések
- [**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.
- [**Műszaki mélyreható** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) – Ez a funkció működésének megismerése.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -gyakran feltett kérdésekre adott válaszokat.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -Útmutató: a szolgáltatással kapcsolatos gyakori problémák megoldása.
- [**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
