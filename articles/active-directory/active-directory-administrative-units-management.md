---
title: "az Azure Active Directoryban aaaAdministrative egységek felügyeleti előzetes verzió"
description: "Adminisztratív egységei használ az Azure Active Directory engedélyeit részletesebb delegálása"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: ee2c7beb6f9f6292bbf3cdeab00801ac066ae0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Az adminisztrációs egységek felügyelete az Azure AD - nyilvános előzetes verzió
Ez a cikk ismerteti adminisztratív egységei – egy új Azure Active Directory-tároló rendszergazdai engedélyek delegálása felhasználók részhalmaza és alkalmazása házirendek tooa részhalmazát felhasználók használható erőforrásokat. Az Azure Active Directoryban a felügyeleti egység központi rendszergazdák toodelegate engedélyek tooregional rendszergazdák vagy tooset házirend alapszinten engedélyezése.

Ez akkor hasznos, a független részlegek, például a nagy egyetemi sok autonóm iskola (üzleti iskolai, műszaki osztály iskolai, és így tovább) egymástól független végzett rendelkező szervezetek számára. Az ilyen osztályok rendelkezik saját informatikai rendszergazdáknak, akik hozzáférést, a felhasználók kezelése és a kifejezetten a felosztás vonatkozó házirendek beállítása. Központi rendszergazdáknak is érdemes toobe képes grant ezen részlegszintű rendszergazdák engedélyek hello felhasználók számára az adott részlegek keresztül. Pontosabban ebben a példában, egy központi felügyeleti segítségével, például egy adott iskolai (üzleti iskolai) a felügyeleti egység létrehozása és feltöltése hello üzleti iskolai felhasználókra. A központi felügyeleti majd hello üzleti iskolai informatikai munkatársak hatókörű tooa szerepkör, más szóval grant hello üzleti iskolai felügyeleti engedélyeket csak keresztül hello iskolai felügyeleti részleg informatikai munkatársak adhat hozzá.

> [!IMPORTANT]
> Felügyeleti egység hatókörbe tartozó rendszergazdai szerepkörök csak akkor, ha engedélyezi a Azure Active Directory Premium rendelhet hozzá. További információkért lásd: [Ismerkedés az Azure AD Premium](active-directory-get-started-premium.md).
>


Hello központi felügyeleti szempontból egy felügyeleti egység hozható létre és töltődik erőforrások címtárobjektum. **Ebben az előzetes kiadásban ezeket az erőforrásokat lehet felhasználókra.** Miután létre és töltődik fel, hello felügyeleti egység egy hatókör toorestrict hello engedélyt csak hello felügyeleti egység található erőforrások keresztül is használható.

## <a name="managing-administrative-units"></a>Felügyeleti egységek kezelése
Ebben az előzetes kiadásban létrehozhat és adminisztratív egységei hello Azure Active Directory modul a Windows PowerShell-parancsmagok használatával kezelheti. toolearn kapcsolatos további által látható toodo [adminisztratív egységei használata](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

További információ a szoftverkövetelmények és hello Azure AD-modul telepítése, és hello Azure AD-modullal parancsmagok adminisztratív egységei, beleértve a szintaxist, a paraméterek leírásait és a példákat, kezelésével kapcsolatos információk [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Következő lépések
[Az Azure Active Directory-kiadások](active-directory-editions.md)
