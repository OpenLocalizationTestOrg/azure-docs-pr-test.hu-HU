---
title: "aaaHow toorequire többtényezős hitelesítéssel |} Microsoft Docs"
description: "Ismerje meg, hogyan toorequire többtényezős hitelesítés (MFA) a kiemelt identitásokat az Azure Active Directory Privileged Identity Management bővítmény hello."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1e3dc4ad-3a6a-4a52-8417-3ca4f84ae05c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c08fad9dc80c09a14a4bd7497da4942fa227c3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorequire-mfa-in-azure-ad-privileged-identity-management"></a>Hogyan toorequire többtényezős hitelesítés az Azure AD Privileged Identity Management
Azt javasoljuk, hogy a rendszergazdák az összes szükséges többtényezős hitelesítés (MFA). Ez csökkenti a hello sérült tooa jelszó miatt a támadás kockázatát.

Megkövetelheti, hogy a felhasználók bejelentkezéskor befejeződnek az MFA-kérdést. blogbejegyzés hello [többtényezős hitelesítés az Office 365 és az Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) összehasonlítja a tartalma Office és az Azure-előfizetések, hello Microsoft Azure multi-factor Authentication ajánlatban foglalt hello szolgáltatást.

Is megkövetelheti, hogy a felhasználók, amikor azok az Azure AD PIM szerepkört aktiváló befejeződnek az MFA-kérdést. Ezzel a módszerrel hello felhasználói nem fejeződött be az MFA-kérdést, ha azok jelentkezett be, ha fogja felszólító toodo által PIM igen.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Az Azure AD Privileged Identity Management a többtényezős hitelesítés megkövetelése
Ha Ön kezeli a PIM egy kiemelt szerepkörű rendszergazda identitások, riasztásokat, amelyek MFA javasoljuk a kiemelt jogosultságokkal rendelkező fiókok jelenhet meg. Kattintson a hello biztonsági riasztás hello PIM irányítópultot és egy új panel nyílik meg kell a többtényezős Hitelesítést igénylő hello rendszergazdai fiókok listáját.  Jelölje ki a több szerepkört, majd kattintson a hello megkövetelheti az MFA **javítsa** gombra, vagy lehet hello folytatást jelző pontokra következő tooindividual szerepkör elemre, és kattintson a hello **javítsa** gombra.

> [!IMPORTANT]
> Most, jobb Azure MFA csak akkor működik a munkahelyi vagy iskolai fiókjait, nem Microsoft-fiókok (általában egy személyes fiók rendelkezik toosign a használt tooMicrosoft szolgáltatásokat, mint a Skype, Xbox, Outlook.com-os, stb.). Ebből kifolyólag bárki, aki a Microsoft-fiókkal nem lehet jogosult rendszergazdája, mert az MFA tooactivate szerepköreik nem használhatják. Ha ezek a felhasználók kezelése a Microsoft-fiókkal munkaterhelések toocontinue, jogosultságszint-emelés őket toopermanent rendszergazdák most.
> 
> 

Emellett hello többtényezős hitelesítés követelménye beállítható egy adott szerepkör módosíthatja a hello hello PIM irányítópult szerepkörök szakasza kattintással. Kattintson a **beállítások** hello szerepkörválasztás paneljét, és jelölje be a **engedélyezése** a többtényezős hitelesítés.

## <a name="how-azure-ad-pim-validates-mfa"></a>Hogyan ellenőrzi az Azure AD PIM a többtényezős hitelesítés
Érvényesítése többtényezős Hitelesítést, amikor egy felhasználó aktiválja a szerepkör esetén két lehetőség áll rendelkezésre.

hello legegyszerűbb lehetőség toorely Azure MFA a felhasználók számára egy kiemelt szerepkörhöz aktiválása esetén. toodo, az első ellenőrizze, hogy ezek a felhasználók licencét, ha szükséges, és az Azure MFA szolgáltatásra regisztrált. További információk hogyan toodo Ez az a [Ismerkedés az Azure multi-factor Authentication hello felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Az ajánlott, de nem szükséges, hogy konfigurálja az Azure AD tooenforce MFA a felhasználók számára, amikor bejelentkeznek a. Ennek az az oka hello MFA fogja ellenőrizni által az Azure AD PIM magát.

Azt is megteheti Ha a felhasználók hitelesítéséhez a helyszíni lehet Identitásszolgáltatóként az MFA felelős. Ha például AD összevonási szolgáltatások toorequire intelligenskártya-alapú hitelesítést az Azure AD elérése előtt már konfigurálta [Securing felhőalapú erőforrásokat az Azure többtényezős hitelesítés és az AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) utasításokat tartalmaz az AD FS toosend jogcímek tooAzure AD konfigurálásához. Ha a felhasználó megpróbál egy szerepkör tooactivate, Azure AD PIM fogad el, hogy többtényezős hitelesítés már érvényesítve lett hello felhasználó után hello megfelelő jogcímeket kap.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

