---
title: "felhasználók és eszközök - Azure MFA kezelésére aaaAdmins |} Microsoft Docs"
description: "Ez ismerteti, hogyan a toochange felhasználói beállítások például hello felhasználók toodo kényszerített hello igazolása felfelé folyamat újra."
documentationcenter: 
services: multi-factor-authentication
author: kgremban
manager: femila
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 8c35435e4f6504014d9a4f6f1b837639c3b53481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-hello-cloud"></a>Azure multi-factor Authentication hello felhő a felhasználói beállítások kezelése
Ha Ön rendszergazda felhasználó-eszköz beállításainak a következő hello kezelheti:

* Felhasználók tooprovide kapcsolattartási módszerek újra megkövetelése
* Alkalmazásjelszók törlése
* Többtényezős hitelesítés megkövetelése minden megbízható eszköz 

## <a name="require-users-tooprovide-contact-methods-again"></a>Felhasználók tooprovide kapcsolattartási módszerek újra megkövetelése
Ezzel a beállítással újra kényszeríti hello felhasználói toocomplete hello regisztrációs folyamatot. Böngészőn kívüli alkalmazások toowork továbbra is, ha hello felhasználó alkalmazásjelszók számukra.  Hello felhasználók alkalmazásjelszók is kiválasztásával törölheti **törli a kijelölt hello felhasználók által létrehozott összes meglévő alkalmazásjelszavak**.

### <a name="how-toorequire-users-tooprovide-contact-methods-again"></a>Hogyan toorequire felhasználók tooprovide kapcsolatba lépni módszerek újra
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldalon válassza ki a **Azure Active Directory** > **felhasználók és csoportok** > **minden felhasználó**.
3. Válassza ki **a multi-factor Authentication**. hello többtényezős hitelesítés lap nyílik meg. 
4. Ellenőrizze a hello mezőben következő toohello felhasználót vagy felhasználókat, hogy kívánja-e toomanage. A Kész lépés beállítások listáját a jobb oldali hello jelennek meg. 
5. Válassza ki **felhasználói beállítások kezelése**.
6. Hello jelölőnégyzetet a **kérése a kiválasztott felhasználóktól tooprovide kapcsolattartási mód újbóli megadásának**.
   ![Adja meg a kapcsolattartási módszerek](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
7. Kattintson a **mentés** gombra.
8. Kattintson a **bezárása**.

## <a name="delete-users-existing-app-passwords"></a>Törölje a meglévő alkalmazásjelszavak felhasználók
Ez a beállítás törli az összes hello alkalmazásjelszókat, amely a felhasználó hozott létre. E alkalmazásjelszókat társított böngészőn kívüli alkalmazások Uj alkalmazasjelszora van szuksege létrehozásáig leáll.

### <a name="how-toodelete-users-existing-app-passwords"></a>Hogyan toodelete felhasználók alkalmazásjelszók meglévő
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldalon válassza ki a **Azure Active Directory** > **felhasználók és csoportok** > **minden felhasználó**.
3. Válassza ki **a multi-factor Authentication**. hello többtényezős hitelesítés lap nyílik meg. 
6. Ellenőrizze a hello mezőben következő toohello felhasználót vagy felhasználókat, hogy kívánja-e toomanage. A Kész lépés beállítások listáját a jobb oldali hello jelennek meg. 
7. Válassza ki **felhasználói beállítások kezelése**.
8. Hello jelölőnégyzetet a **törli a kijelölt hello felhasználók által létrehozott összes meglévő alkalmazásjelszavak**.
   ![Alkalmazásjelszók törlése](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Kattintson a **mentés** gombra.
10. Kattintson a **bezárása**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Többtényezős hitelesítés visszaállítása egy felhasználó összes megjegyzett eszközön
Az Azure multi-factor Authentication hello konfigurálható szolgáltatásait a rendelkezésre állást a felhasználók hello beállítás toomark eszközök megbízhatóként. További információkért lásd: [konfigurálása Azure multi-factor Authentication beállításainak](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Felhasználók is kikapcsolja a kétlépéses ellenőrzést, a konfigurálható számú nappal szabályos eszközeiken. Ha egy fiók biztonsága sérül, vagy megbízható eszköz elvész, ha toobe képes tooremove megbízható hello állapot kell, és kétlépéses ellenőrzés visszakapcsolásához.

Hello **visszaállítási többtényezős hitelesítés az összes korábban megjegyzett eszközön** beállítás azt jelenti, hogy a felhasználó hello kifogásolt tooperform kétlépéses ellenőrzés hello következő bejelentkezéskor, függetlenül attól, hogy kiválasztott kategóriának toomark a eszköz megbízható legyen. 

### <a name="how-toorestore-mfa-on-all-suspended-devices-for-a-user"></a>Hogyan toorestore többtényezős hitelesítés a felhasználó az összes felfüggesztett eszközön
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldalon válassza ki a **Azure Active Directory** > **felhasználók és csoportok** > **minden felhasználó**.
3. Válassza ki **a multi-factor Authentication**. hello többtényezős hitelesítés lap nyílik meg. 
6. Ellenőrizze a hello mezőben következő toohello felhasználót vagy felhasználókat, hogy kívánja-e toomanage. A Kész lépés beállítások listáját a jobb oldali hello jelennek meg. 
7. Válassza ki **felhasználói beállítások kezelése**.
8. Hello jelölőnégyzetet a **visszaállítási többtényezős hitelesítés az összes korábban megjegyzett eszközön**
   ![alkalmazásjelszók törlése](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Kattintson a **mentés** gombra.
10. Kattintson a **bezárása**.

## <a name="next-steps"></a>Következő lépések

- További információért arról, hogyan túl[konfigurálása Azure multi-factor Authentication beállításait](multi-factor-authentication-whats-next.md)

- Ha a felhasználók segítségre van szüksége, irányítsa őket felé hello [felhasználói útmutatója a kétlépéses ellenőrzéshez](./end-user/multi-factor-authentication-end-user.md)
