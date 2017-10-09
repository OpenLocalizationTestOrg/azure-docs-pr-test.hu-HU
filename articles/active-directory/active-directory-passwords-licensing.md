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
ms.openlocfilehash: 9cecaaac429165346f7082f1965dc8a21063fe7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Licencelési követelmények az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása

Ahhoz, hogy az Azure AD jelszó-átállítási toofunction hogy **rendelkeznie kell legalább egy licenc nélküli a szervezet**. Nem határoztunk felhasználónkénti hello jelszó alaphelyzetbe állítása során szerzett licencelési. a Microsoft-licencszerződés toomaintain betartását, tooassign licencek tooany felhasználók prémium szolgáltatások használatáért kell.

* **Csak a felhasználók felhőalapú** -Office 365 (Office 365) bármely fizetett SKU vagy Azure AD alapvető
* **Felhő** és/vagy **helyszíni felhasználók** -Azure AD Premium P1 vagy P2, Enterprise Mobility + Security (EMS) vagy biztonságos hatékony vállalati (Másodper)

## <a name="licenses-required-for-password-writeback"></a>A jelszóvisszaírás szükséges engedélyek

toouse jelszóvisszaírás, rendelkeznie kell a következő bérlőben licenccel hello egyikét.

* Prémium szintű Azure AD P1
* Prémium szintű Azure AD P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Önálló Office 365 tervek licencelési **nem támogatják a jelszóvisszaírás** és jelszó használata kötelezővé tehető a funkciók toowork tervei megelőző hello.

További licencelési adatait, beleértve a költségek található meg a következő lapok hello

* [Az Azure Active Directory árképzési hely](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Biztonságos hatékony Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a>Csoport vagy felhasználó alapú licencelés engedélyezése

Mostantól az Azure AD támogatja a csoport-alapú tömeges tooa csoport számára licencek, így a rendszergazdák tooassign licencelési, mint az őket egyenként. [Rendelje hozzá, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Licenc tooa felhasználói rendelhető, mielőtt hello rendszergazda hello felhasználói kell adnia hello "Használat helye" tulajdonság. Licenc hozzárendelése felhasználói végezhető > Profil > Bővítménybeállítási szakasza hello Azure-portálon. **Licenc-hozzárendelést használ, anélkül, hogy a megadott felhasználási hely összes felhasználója örökli hello könyvtár hello helye.**

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható

