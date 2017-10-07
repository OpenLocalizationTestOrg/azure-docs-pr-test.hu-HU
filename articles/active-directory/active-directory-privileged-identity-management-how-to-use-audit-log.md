---
title: "az Azure AD Privileged Identity Management aaaHow toouse hello audit napló |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello napló hello Azure Privileged Identity Management bővítmény."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 36987eaab9fe02c5dd7b4f4705e487299430745d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-audit-log-in-pim"></a>A PIM hello napló használatával
Használhatja hello Privileged Identity Management (PIM) naplózási napló toosee összes hello felhasználó hozzárendelést és aktiválások egy adott időtartamon belül. Ha azt szeretné, toosee hello teljes naplózási előzmények tevékenység a bérlői rendszergazda, a végfelhasználó és a szinkronizálási tevékenység, beleértve a hello használhatja [Azure Active Directory hozzáférési és használati jelentések.](active-directory-view-access-usage-reports.md)

## <a name="navigate-toohello-audit-log"></a>Keresse meg a toohello audit napló
A hello [Azure-portálon](https://portal.azure.com) irányítópultot, válassza hello **Azure AD Privileged Identity Management** alkalmazást. Ott, hozzáférési napló hello kattintva **kiemelt szerepköröket kezelése** > **naplózási előzmények** hello PIM irányítópulton.

## <a name="hello-audit-log-graph"></a>hello naplózási napló diagramhoz
Hello naplózási napló tooview hello összes aktiválás, napi maximális aktiválások és napi átlagos aktiválások vonaldiagramon használható.  Hello adatok szerepkör szűrhető Ha több szerepkör hello naplózási előzmények szerepel.

Használjon hello **idő**, **művelet**, és **szerepkör** gombok toosort hello napló.

## <a name="hello-audit-log-list"></a>hello naplózási naplók listája
a hello napló napló listájában hello oszlopok a következők:

* **Kérelmező** -hello felhasználó, aki a kért hello szerepkör aktiválása vagy módosítása.  Hello értéke "Azure rendszer", ha a naplóban hello Azure naplózási további információt.
* **Felhasználói** -hello felhasználó az aktiválás vagy tooa szerepkörrel rendelkeznek.
* **Szerepkör** -hello szerepkör vagy felhasználó hello aktiválására.
* **A művelet** – hello kérelmező hello műveleteit. Ez magában foglalhatja hozzárendelés, helyekhez, aktiválás vagy deaktiválás.
* **Idő** – Ha hello művelet történt.
* **Mintafelismerési** -szöveg hello OK mezőbe az aktiválás során megadása esetén megjelenik itt.
* **Lejárati** – csak akkor érvényes, a szerepkörök aktiválásához.

## <a name="filter-hello-audit-log"></a>Szűrő hello audit napló
Hello információkat megjelenő hello naplóban hello kattintva szűrheti **szűrő** gombra.  Hello **frissítés diagram (paraméterek) panelen** jelenik meg.

Hello szűrők beállítását követően kattintson **frissítés** toofilter hello adatok hello naplóban.  Ha hello adatok nem jelennek meg azonnal, frissítse a hello lapot.

### <a name="change-hello-date-range"></a>Hello dátumtartomány módosítása
Használjon hello **Ma**, **múlt héten**, **elmúlt hónap**, vagy **egyéni** toochange hello időtartománya hello napló gomb.

Ha úgy dönt, hogy hello **egyéni** gomb, Ön által megadott egy **a** dátummezője és egy **való** dátum mező toospecify dátumtartomány hello naplóhoz.  Hello dátumát éééé/hh/nn formátumban adja meg, vagy kattintson a hello **naptár** ikonra, és válassza a hello dátumát a naptárból.

### <a name="change-hello-roles-included-in-hello-log"></a>Hello szerepköreinek hello napló módosítása
Ellenőrizze, vagy törölje a jelet hello **szerepkör** jelölőnégyzet következő tooeach szerepkör tooinclude vagy kizárási azt hello jelentkezzen.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

