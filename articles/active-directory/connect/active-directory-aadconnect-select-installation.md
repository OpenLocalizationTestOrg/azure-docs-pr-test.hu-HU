---
title: "Az Azure AD Connect: A telepítés típusának kiválasztása |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan hogyan tooselect hello telepítési adja meg az Azure AD Connect toouse"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a700e59eb05947ee1dbd9993141200c9340b2f1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="select-which-installation-type-toouse-for-azure-ad-connect"></a>Az Azure AD Connect melyik telepítési típus toouse kiválasztása
Az Azure AD Connect két tartozik telepítése új telepítés: Express és testre szabható. Ez a témakör segít toodecide, amely toouse lehetőség a telepítés során.

## <a name="express"></a>Express
Express hello leggyakrabban használt beállítás, és minden új telepítések körülbelül 90 %-át használja. Tervezett tooprovide hello leggyakoribb forgatókönyvet a megfelelő konfigurációt volt.

Ez azt feltételezi, hogy:

- A helyi erdő egy egyetlen Active Directory rendelkezik.
- Hello telepítéséhez használhatja a vállalati rendszergazdai fiókkal rendelkezik.
- A helyszíni Active Directoryban kisebb, mint 100 000 objektummal rendelkezik.

Elérhetővé:

- [A jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) a helyszíni tooAzure AD az egyszeri bejelentkezést.
- A konfiguráció, amely szinkronizálja [felhasználók, csoportok, névjegyek és Windows 10-es számítógépeken](active-directory-aadconnectsync-understanding-default-configuration.md).
- Az összes tartomány és az összes szervezeti egység összes jogosult objektumok szinkronizálás.
- [Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) van engedélyezett toomake mindig kell használnia, hello elérhető legújabb verzióra.

Ha továbbra is használhatja az expressz beállításokat:

- Ha nem szeretné, hogy toosynchronize minden hozzá, továbbra is használhat Express és hello utolsó lapján kijelölésének **hello szinkronizálási folyamat indítása...** *. Ezután futtassa újra a hello telepítővarázslóját, és módosítsa a hello szervezeti egységek az [konfigurációs beállítások](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) és ütemezett szinkronizálás engedélyezése.
- Azt szeretné, például a jelszóvisszaírást az Azure AD Premium hello funkcióinak egyike a tooenable. Először halad át expressz tooget hello kezdeti telepítése befejeződött. Ezután futtassa újra a hello telepítővarázslóját, és módosítsa a hello [konfigurációs beállítások](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## <a name="custom"></a>Egyéni
hello egyéni elérési út lehetővé teszi, hogy számos további lehetőség express-nál. Minden olyan esetben, ahol az express előző szakaszban leírt hello konfigurációs nincs a szervezet reprezentatív használandó.

A következő esetekben használja:

- Önnek nincs hozzáférési tooan vállalati rendszergazdai fiók az Active Directoryban.
- Több erdővel rendelkezik, vagy egynél több erdő a jövőbeli hello toosynchronize tervezi.
- Az erdő hello Connect-kiszolgáló nem elérhető tartományok vannak.
- Tervezi toouse összevonási vagy a felhasználói bejelentkezés átmenő hitelesítés.
- Több mint 100 000 objektummal rendelkezik, és toouse teljes SQL Server szükséges.
- Tervezi toouse csoport alapú szűrés és nem csak tartomány vagy szervezeti egység alapú szűrés.

## <a name="upgrade-from-dirsync"></a>Frissítés a DirSync szolgáltatásról
Ha a DirSync jelenleg használ, majd kövesse hello [frissítésre a Dirsyncről](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade a meglévő konfigurációt. Két különböző frissítési lehetőségek közül:

- Helybeni verziófrissítés tooinstall Csatakoznia hello azonos kiszolgálón.
- Párhuzamos központi telepítés tooinstall Connect egy új kiszolgálón hello működő DirSync-kiszolgálóval pedig továbbra is működik.

## <a name="upgrade-from-azure-ad-sync"></a>Frissítés Azure AD-szinkronizálóról
Ha Azure AD Sync jelenleg használ, akkor hajtsa végre az hello [ugyanazokat a lépéseket](active-directory-aadconnect-upgrade-previous-version.md) , amikor frissít egy csatlakozás verziója tooa újabb a. Két különböző frissítési lehetőségek közül:

- Helybeni verziófrissítés tooinstall Csatakoznia hello azonos kiszolgálón.
- Mozgó-áttelepítési tooinstall Connect hello meglévő Azure AD Sync server során egy új kiszolgálón továbbra is működik.

## <a name="migrate-from-fim2010-or-mim2016"></a>FIM2010 vagy MIM2016 áttelepítése
Ha a Forefront Identity Manager 2010 vagy a Microsoft Identity Manager 2016 az Azure AD-összekötő hello jelenleg használ, majd egyetlen választása marad: áttelepítés. Kövesse az ismertetett lépések hello [mozgó-áttelepítési](active-directory-aadconnect-upgrade-previous-version.md#swing-migration). Cserélje le az Azure AD Sync bármely hashtagként hello lépéseket FIM2010/MIM2016.

## <a name="next-steps"></a>Következő lépések
Attól függően, hogy hello lehetőséget választotta a toouse, hello táblázat a tartalom toohello bal oldali toofind hello a cikk részletes lépéseket.
