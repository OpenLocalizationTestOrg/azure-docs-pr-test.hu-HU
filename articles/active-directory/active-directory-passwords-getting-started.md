---
title: "Gyors útmutató: Azure AD SSPR | Microsoft Docs"
description: "Az Azure AD önkiszolgáló jelszó-visszaállításának gyors üzembe helyezése"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 4fed3a1c690fd6423ee5d3e5baef690d8896fbe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>Gyors útmutató: Az Azure AD új jelszó kérésére vonatkozó önkiszolgáló folyamata

> [!IMPORTANT]
> **Azért van itt, mert problémák merültek fel a bejelentkezéssel kapcsolatban?** Ha igen, [így módosíthatja vagy állíthatja alaphelyzetbe a jelszavát](active-directory-passwords-update-your-own-password.md).

## <a name="rapidly-deploy-self-service-password-reset"></a>Az önkiszolgáló jelszó-visszaállítás gyors üzembe helyezése

Az önkiszolgáló jelszó-változtatási (SSPR) ajánlatok egy egyszerű azt jelenti, hogy az informatikai rendszergazdák tooempower felhasználók tooreset, vagy a jelszavak és a fiókok zárolásának feloldásához. hello rendszer tartalmaz részletes jelentéskészítési tootrack hello rendszerről és értesítések tooalert használatakor Ön toomisuse vagy való visszaélés.

Ez a tájékoztató feltételezi, hogy már rendelkezik egy Azure AD-bérlő érvényes próbaverziójával vagy licencével. Ha az Azure AD beállítása segítségre van szüksége, tekintse meg hello [Ismerkedés az Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

1. A meglévő Azure AD-bérlőjében válassza ki a **„Jelszó átállítása”** lehetőséget.

2. A hello **"Tulajdonságok"** képernyő, a "Self Service jelszó alaphelyzetbe állítása kompatibilis" alábbi lehetőségek közül választhat hello hello beállítás
    * Senki sem - nem egy, képes toouse SSPR funkció
    * A csoport - egy adott Azure AD csak tagjai csoportot választani képes toouse SSPR funkciók vannak
    * Mindenki - fiókkal az Azure AD-bérlő rendelkező összes felhasználó képes toouse SSPR funkciók

3. A hello **"Hitelesítési módszerek"** képernyőn válassza a
    * Számos módszer szükséges tooreset – legalább egy vagy két legfeljebb támogatott
    * Módszerek elérhető toousers - kell legalább egy, de soha nem hurts a toohave egy további lehetőség
        * **E-mailek** küld e-mailben található egy kódot toohello felhasználó beállított hitelesítési e-mail cím
        * **Mobiltelefon** által biztosított felhasználói hello választott tooreceive hívás hello, vagy a kód tootheir szöveg konfigurált Mobil telefonszám
        * **Irodai telefon** hívások hello felhasználót egy kódot tootheir irodai telefonszámát konfigurálva
        * **Biztonsági kérdések** toochoose használatához
            * Kérdések számát kötelező tooregister, – hello minimális sikeres regisztráció, ami azt jelenti, a felhasználó választhat tooanswer további toocreate készletét a toopull kérdésekre. Ezt a beállítást a 3-5 állítható be kell és lehet nagyobb, mint vagy egyenlő kérdések szükséges tooreset toohello száma is.
                * Biztonsági kérdések kiválasztásakor hello "Egyéni" gombra kattintva felveheti egyéni kérdések
            * Kötelező kérdések számát tooreset - állíthat be a 3-5 kérdések toobe, mielőtt engedélyezi a felhasználók jelszó alaphelyzetbe állítása vagy feloldott legyen toobe helyesen válaszolni.

4. AJÁNLOTT: **"Testreszabási"** lehetővé teszi a toochange hello "Forduljon a rendszergazdához" hivatkozás toopoint tooa lap vagy e-mail címet adhat meg

5. Választható lehetőség: hello **"Regisztrációs"** képernyő hello lehetőséget biztosít a rendszergazdák:
    * Felhasználók tooregister kérése, amikor a bejelentkezés
    * Hány nap elteltével a felhasználó információt kér a rendszer tooreconfirm a hitelesítés

6. Választható lehetőség: hello **"Értesítések"** képernyő rendszergazdák hello lehetőségeket kínálja:
    * Értesítse a felhasználókat új jelszó kérésekor?
    * Minden rendszergazda kapjon értesítést, ha más rendszergazdák új jelszót kérnek?

**Most már konfigurálta az SSPR funkciót Azure AD-bérlője számára**. Itt leállítása vagy a Folytatás tooconfigure a jelszavak tooan szinkronizálása a helyszíni Active Directory-tartománynak.

> [!NOTE]
> Az SSPR-t egy felhasználóval tesztelje, ne egy rendszergazdával, mert a Microsoft szigorú hitelesítési előírásokat tartat be az Azure rendszergazdai típusú fiókjaihoz. Hello rendszergazdai jelszó házirend kapcsolatos további információkért lásd: a [jelszó házirend cikk](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-tooexisting-identity-source"></a>Konfigurálja a szinkronizálási tooexisting identitás forrás

tooenable a helyszíni identitás szinkronizálási tooAzure AD, akkor tooinstall kell, és beállíthatja [az Azure AD Connect](./connect/active-directory-aadconnect.md) a szervezet a kiszolgálón. Ez az alkalmazás kezeli a szinkronizálási felhasználókat és csoportokat a meglévő identitás forrás tooyour Azure AD-bérlő.

* [AD Connect a DirSync vagy az Azure AD Sync tooAzure verzióról](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Első lépések az Azure AD Connecttel a gyorsbeállítások használatával](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurálja a jelszóvisszaírás](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite jelszavakat az Azure AD biztonsági tooyour helyszíni címtárba.

## <a name="disabling-self-service-password-reset"></a>Önkiszolgáló jelszóátállítás letiltása

Önkiszolgáló jelszóváltoztatás letiltása, akkor egyszerűen az Azure AD-bérlő megnyitása túl is**jelszó-átállítási > Tulajdonságok** > Válasszon **nincs** alatt **önkiszolgáló jelszó-változtatási Engedélyezve**

### <a name="learn-more"></a>Részletek
a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés is megtanulta, hogyan tooconfigure önkiszolgáló jelszó-átállítási a felhasználók számára. hajtsa végre az alábbi lépéseket az Azure portál toocomplete toocontinue toohello hello toohello portál alábbi hivatkozását.

> [!div class="nextstepaction"]
> [Önkiszolgáló jelszóátállítás engedélyezése](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

