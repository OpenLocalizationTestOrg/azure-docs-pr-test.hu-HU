---
title: "Bevezetés: Azure AD SSPR | Microsoft Docs"
description: "Tippek az Azure AD önkiszolgáló jelszóátállítás sikeres bevezetéséhez"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 73d31679b38ff009a767335adaebc49fbc5a75b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="roll-out-password-reset-for-users"></a>Jelszóátállítás bevezetése felhasználók számára

A legtöbb ügyfél hello lépésekkel tooensure SSPR funkciókat biztosítanak egy zökkenőmentes bevezetés kövesse.

1. [A jelszóátállítás engedélyezése a címtárban](active-directory-passwords-getting-started.md)
2. [A helyszíni AD-engedélyeket konfigurálása a jelszóvisszaíró számára](active-directory-passwords-how-it-works.md#active-directory-permissions)
3. [Konfigurálja a jelszóvisszaírás](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite jelszavakat az Azure AD biztonsági tooyour helyszíni címtár
4. [A szükséges licencek hozzárendelése és ellenőrzése](active-directory-passwords-licensing.md)
5. Ha azt szeretné, hogy tooroll kimenő fokozatosan, akkor is opcionálisan korlátot jelszó-átállítási felhasználók tooroll hello szolgáltatás kimenő tooa csoportja lassan adott idő alatt. toodo a beállított hello **Self Service jelszó alaphelyzetbe állítása engedélyezve** a Váltás **mindenki** túl**csoport** válassza ki a biztonsági csoport tooenable jelszó-visszaállításhoz. hello a csoport tagjai az összes rendelkeznie kell licenccel toothem, és egy nagy tooenable [csoport alapú licencelési](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing).
6. Hello minimálisan feltöltése [hitelesítési adatok](active-directory-passwords-data.md)a házirend alapján.
7. Mutatja meg a felhasználók hogyan toouse SSPR, küldésével utasításokat tooshow azokat hogyan tooregister és hogyan tooreset.
    > [!NOTE]
    > Az SSPR-t egy felhasználóval tesztelje, ne egy rendszergazdával, mert a Microsoft szigorú hitelesítési előírásokat tartat be az Azure rendszergazdai típusú fiókjaihoz. Hello rendszergazdai jelszó házirend kapcsolatos további információkért lásd: a [részletes bemutatója a cikk](active-directory-passwords-how-it-works.md).

8. Tooenforce regisztrációs bármikor kiválaszthatja, és igényelnek a felhasználók tooreconfirm a hitelesítési adatokat egy bizonyos idő után. Ha nem szeretné, a felhasználók toohave tooregister, akkor [központi telepítése a jelszó alaphelyzetbe állítása, anélkül, hogy a végfelhasználói regisztrálási](active-directory-passwords-data.md).
9. Adott idő alatt, tekintse át a felhasználók regisztrálása és hello megtekintésével használatával [az Azure AD által biztosított jelentéskészítési](active-directory-passwords-reporting.md).

## <a name="email-based-rollout"></a>E-mailes alapú bevezetés

Sok ügyfél találja egy e-mailek kampány egyszerű toouse utasításokkal hello legegyszerűbb módja tooget felhasználók toouse SSPR. [Létrehoztunk három egyszerű e-maileket a bevezetést sablonok toohelp pontként használható.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* **Hamarosan** e-mail sablon toobe hello hét vagy nap elteltével bevezetés toolet felhasználókkal, hogy valami szükségük toodo használja.
* **Most már elérhető** e-mail sablon használt toobe hello indítási toodrive felhasználók tooregister napját és, hogy használhassák SSPR, amikor szükség, győződjön meg arról, hogy a hitelesítési adatokat.
* **Iratkozzon fel emlékeztető** e-mail sablon néhány napon tooweeks a központi telepítés tooremind felhasználók tooregister után, és erősítse meg a hitelesítési adatokat.

## <a name="creating-your-own-password-portal"></a>Saját jelszókezelő portál létrehozása

Nagyobb ügyfeleink számos toohost weblap válassza ki, és hozzon létre egy legfelső szintű DNS-bejegyzés, például a https://passwords.contoso.com. Ezek feltöltése ezen a lapon, a hivatkozások toohello az Azure AD-jelszó alaphelyzetbe állítása, jelszó-átállítási regisztráció, a jelszó módosítása portálok és a más szervezet-specifikus adatait. Bármilyen e-mail kommunikációt vagy szórólapok küld ki, majd telepíthet is vállalati arculattal ellátott, de könnyen megjegyezhető, URL-CÍMÉT, hogy a felhasználók folytathatja toowhen toouse hello szolgáltatások van szükségük.

* Jelszóátállítási portál – https://passwordreset.microsoftonline.com
* Jelszóátállítási regisztrációs portál – http://aka.ms/ssprsetup
* Jelszómódosítási portál – https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## <a name="using-enforced-registration"></a>Kényszerített regisztráció használata

Ha a felhasználók tooregister jelszó-visszaállításhoz, kényszerítheti őket tooregister bejelentkeznek az Azure AD használata ingyenes. Engedélyezheti ezt a beállítást a címtárban lévő **jelszó-átállítási** hello engedélyezésével panel **bejelentkezéshez szükséges felhasználók tooRegister** hello beállítást **regisztrációs** a lapon.

Is rendszergazdáinak felhasználók toore-regisztráció egy bizonyos idő eltelte után hello beállítása által **számú nap elteltével a felhasználó információt kér a rendszer tooreconfirm a hitelesítési** 0-730 között nap.

Ez a beállítás engedélyezését követően bejelentkező felhasználók üzenet jelenik meg, amely közli a rendszergazda megköveteli őket tooverify hitelesítési adataikat.

## <a name="populate-authentication-data"></a>Hitelesítési adatok feltöltése

Ha meg [a felhasználók számára a hitelesítési adatok feltöltése](active-directory-passwords-data.md), majd a felhasználóknak nem kell tooregister a jelszó alaphelyzetbe állítása előtt képes toouse SSPR. Mindaddig, amíg a felhasználóknál hello hitelesítési adatok definiálva, amely megfelel a megadott hello jelszó-visszaállítási házirend, a felhasználók képesek tooreset a jelszavukat.

## <a name="disabling-self-service-password-reset"></a>Önkiszolgáló jelszóátállítás letiltása

Önkiszolgáló jelszóváltoztatás letiltása, akkor egyszerűen az Azure AD-bérlő megnyitása túl is**jelszó-átállítási**, **tulajdonságok**, kiválasztásával **senki sem** alatt **Önkiszolgáló jelszó-visszaállítás engedélyezése**

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható
