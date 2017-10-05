---
title: "Licencelés: Azure AD SSPR |} Microsoft Docs"
description: "Az Azure AD az önkiszolgáló jelszó-átállítási licencelési követelmények"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 936134bddad19964f809a17f200ebbeed5aa853c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Licencelési követelmények az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása

Ahhoz, hogy Azure AD-Jelszóvisszaállítási függvény akkor **rendelkeznie kell legalább egy licenc nélküli a szervezet**. Nem határoztunk felhasználónkénti a jelszó alaphelyzetbe állítása során szerzett licencelési. A Microsoft licencszerződésben a megfelelőség biztosítása érdekében, licenceket rendelhet azokat a felhasználókat, a prémium szolgáltatások használatáért kell.

* **Csak a felhasználók felhőalapú** -Office 365 (Office 365) bármely fizetett SKU vagy Azure AD alapvető
* **Felhő** és/vagy **helyszíni felhasználók** -Azure AD Premium P1 vagy P2, Enterprise Mobility + Security (EMS) vagy biztonságos hatékony vállalati (Másodper)

## <a name="licenses-required-for-password-writeback"></a>A jelszóvisszaírás szükséges engedélyek

A jelszóvisszaírás használatához rendelkeznie kell egyet a következő licenccel az Ön bérelt szolgáltatásának.

* Prémium szintű Azure AD P1
* Prémium szintű Azure AD P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Önálló Office 365 tervek licencelési **nem támogatják a jelszóvisszaírás** és jelszó használata kötelezővé tehető az előző csomagok esetében ez a funkció működéséhez.

A következő oldalakon található további licencelési adatait, beleértve a költségek

* [Az Azure Active Directory árképzési hely](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Biztonságos hatékony Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a>Csoport vagy felhasználó alapú licencelés engedélyezése

Mostantól az Azure AD támogatja a biztonságicsoport-alapú licencelés engedélyezése rendszergazdák számára, hogy egyszerre több licenceket rendelhet egy felhasználói csoporton, mint az őket egyenként. [Rendelje hozzá, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Licenc rendelhet egy felhasználót, mielőtt a rendszergazda a felhasználó a "Használati hely" tulajdonság adjon meg. Licenc hozzárendelése felhasználói végezhető > Profil > Beállítások szakaszban az Azure portálon. **Licenc-hozzárendelést használ, anélkül, hogy a megadott felhasználási hely összes felhasználója örökli a könyvtár helye.**

## <a name="next-steps"></a>Következő lépések

Az alábbi hivatkozásokat követve az Azure AD jelszóátállításáról olvashat további információkat.

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Adatok**](active-directory-passwords-data.md) – A szükséges adatok megismerése, és az adatok használata a rendszer jelszókezelésre.
* [**Bevezetés**](active-directory-passwords-best-practices.md) – Az SSPR funkció tervezése és üzembe helyezése a felhasználók számára az itt található útmutató segítségével.
* [**Testreszabás**](active-directory-passwords-customize.md) – Az SSPR-felület megjelenésének és működésének testre szabása a cége számára.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Részletes műszaki bemutatás**](active-directory-passwords-how-it-works.md) – Egy pillantás a függöny mögé, hogy megértse, hogyan is működik.
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? – Válaszok olyan kérdésekre, amiket mindig is fel akart tenni
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – Ismerje meg, hogyan oldhat meg általános, az SSPR működése során jelentkező hibákat.

