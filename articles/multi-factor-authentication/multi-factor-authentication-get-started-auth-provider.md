---
title: "aaaGet lépések az Azure többtényezős hitelesítésszolgáltató |} Microsoft Docs"
description: Ismerje meg, hogy az Azure multi-factor Auth Provider toocreate.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 00ea967a80b43baff38c1de586c54d95c9abac2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Első lépések az Azure Multi-Factor Auth szolgáltatóval
A kétlépéses ellenőrzés alapértelmezés szerint elérhető az Azure Active Directory- és Office 365-felhasználókkal rendelkező globális adminisztrátorok számára. Azonban ha tootake előnyeit [speciális szolgáltatások](multi-factor-authentication-whats-next.md) majd hello teljes változatát, az Azure multi-factor Authentication (MFA) kell vásárolnia.

Az Azure multi-factor Auth Provider használt tootake hello teljes változatát, az Azure MFA által nyújtott szolgáltatások előnyeit. Ez olyan felhasználóknak készült, akik **nem rendelkeznek Azure MFA, Azure AD Prémium vagy Enterprise Mobility + Security (EMS) licenccel**.  Az Azure MFA, prémium szintű Azure AD és az EMS tartalmazza hello teljes verzióját az Azure MFA alapértelmezés szerint. Ha rendelkezik licencekkel, akkor nincs szüksége Azure Multi-Factor Auth szolgáltatóra.

Az Azure multi-factor Auth provider szükséges toodownload hello SDK.

> [!IMPORTANT]
> toodownload hello SDK, kell, hogy az Azure multi-factor Auth Provider toocreate akkor is, ha az Azure MFA-, prémium szintű, vagy az EMS licenccel rendelkezik.  Hozzon létre egy Azure többtényezős hitelesítésszolgáltató erre a célra, és már rendelkezik licenccel, ha kell, hogy toocreate hello szolgáltató a hello **Per Enabled User** modell. Ezt követően kapcsolja hello szolgáltató toohello tartalmazó könyvtár hello Azure MFA, a prémium szintű Azure AD vagy az EMS-licenceket. Ez a konfiguráció biztosítja, hogy Ön csak számlázása Amennyiben több egyedi felhasználók hajt végre a kétlépéses ellenőrzést, mint amennyit vásárolt licencek hello száma.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Mi az az Azure Multi-Factor hitelesítési szolgáltató?

Azure multi-factor Authentication szempontjából nincs licencek, ha a felhasználók számára is létrehozhat egy hitelesítési szolgáltató toorequire kétlépéses ellenőrzést. Ha egy egyéni alkalmazást fejleszt, és az Azure MFA tooenable, hozzon létre egy hitelesítésszolgáltató és [hello SDK letöltése](multi-factor-authentication-sdk.md).

Hitelesítésszolgáltatók két típusa van, és hello különbséget körül hogyan az Azure-előfizetés fel van töltve. hello hitelesítési beállítás kiszámítja a bérlő hajt végre a hónap hitelesítések hello számát. Ez a lehetőség akkor a legjobb, ha néhány felhasználó csak alkalmanként végez hitelesítést, mint amikor az MFA-ra egyéni alkalmazáshoz van szüksége. hello felhasználói beállítás kiszámítja az Ön bérelt szolgáltatásának használhatják, akik a kétlépéses ellenőrzést végrehajtani egy hónap hello számát. Ez a beállítás akkor ajánlott, ha néhány licenccel rendelkező felhasználók rendelkezik, de a licencelési korlátozások túl tooextend MFA toomore felhasználók kell.

## <a name="create-a-multi-factor-auth-provider"></a>Multi-Factor Auth szolgáltató létrehozása
A következő lépéseket toocreate az Azure multi-factor Auth Provider hello használata. Az Azure többtényezős hitelesítésszolgáltatók csak a klasszikus Azure portálon hello hozhatók létre. Ha nem tud bejelentkezni a klasszikus Azure portálon toohello, ellenőrizze a toomake arról, hogy az Azure AD-bérlő [Azure-előfizetéssel társított](../active-directory/active-directory-how-subscriptions-associated-directory.md). 

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) rendszergazdaként.
2. Hello bal oldalon válassza ki a **Active Directory**.
3. Hello Active Directory lap hello lap tetején jelölje be a **többtényezős hitelesítési szolgáltatók**.
   
   ![MFA-szolgáltató létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. Hello alul kattintson **új**.
   
   ![MFA-szolgáltató létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. Az App Services alatt válassza a **Multi-Factor Auth szolgáltató** lehetőséget
   
   ![MFA-szolgáltató létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Kattintson a **Gyors létrehozás** gombra.
   
   ![MFA-szolgáltató létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Adja meg a következő mezők hello, és válassza ki **létrehozása**.
   1. **Név** – hello hello többtényezős hitelesítésszolgáltató neve.
   2. **Használati modell** – Válassza az alábbi két lehetőség egyikét:
      * Hitelesítésenként – Hitelesítésenként díjat felszámító vásárlási modell. Általában ügyfél felől elérhető alkalmazásokban Azure Multi-Factor Authentication hitelesítést használó forgatókönyvekben használják.
      * Engedélyezett felhasználónként – Felhasználónként díjat felszámító vásárlási modell. Általában alkalmazott hozzáférés tooapplications például az Office 365 esetében használatos. Akkor válassza ezt a beállítást, ha már van néhány Azure MFA-licenccel ellátott felhasználója.
   3. **Directory** – hello Azure Active Directory-bérlőben adott hello többtényezős hitelesítésszolgáltató társítva. Vegye figyelembe a következőket hello:
      * Nem kell egy Azure Active directory toocreate a többtényezős hitelesítésszolgáltató. Ezt a jelölőnégyzetet hagyja üresen, ha csak toodownload hello Azure multi-factor Authentication kiszolgáló vagy az SDK-val.
      * Többtényezős hitelesítésszolgáltató hello egy speciális szolgáltatások hello Azure Active directory tootake előnyeit társítani kell.
      * Egy Azure AD-címtárral csak egyetlen Multi-Factor Auth szolgáltató társítható.  
      ![MFA-szolgáltató létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Gombra kattintva hozzon létre, a multi-factor Authentication hitelesítésszolgáltató létrehozása hello és meg kell megjelennie a következő üzenet: **sikeresen létrejött a többtényezős hitelesítésszolgáltató**. Kattintson az **OK** gombra.  
   
   ![MFA-szolgáltató létrehozása](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-multi-factor-auth-provider"></a>A Multi-Factor Auth szolgáltató kezelése

Nem módosítható hello használati modell (engedélyezett felhasználónkénti vagy hitelesítésenkénti) az MFA-szolgáltatóra létrehozása után. Azonban hello többtényezős hitelesítési szolgáltató törlése, és ezután létrehozhat egyet a különböző használati modell.

Ha hello aktuális többtényezős hitelesítésszolgáltató társítva (más néven Azure AD-bérlő) Azure AD-címtárral, biztonságosan hello többtényezős hitelesítési szolgáltató törlése és hozzon létre egyet csatolt toohello ugyanazt az Azure AD-bérlő. Azt is megteheti Ha vásárolt elég MFA, Azure AD Premium vagy Enterprise Mobility + Security (EMS) licencet toocover minden felhasználó többtényezős hitelesítés engedélyezett, törölheti hello többtényezős hitelesítési szolgáltató regisztrálását.

Ha a többtényezős hitelesítési szolgáltató nem csatolt tooan az Azure AD bérlői, vagy csatolunk hello új többtényezős hitelesítési szolgáltató tooa másik Azure AD bérlői, felhasználói beállítások és a konfigurációs beállítások nem kell másolnia. Is az Azure MFA kiszolgáló meglévő kell újraaktiválni során generált aktiválási hitelesítő adatok használatával toobe hello új MFA-szolgáltató. Hello multi-factor Authentication kiszolgálók toolink újraaktiválhatja azokat toohello új MFA-szolgáltató nem befolyásolja a telefonhívás és a szöveges üzenet hitelesítési, de a mobilalkalmazáson keresztüli értesítések működése leáll az összes felhasználó számára addig, amíg azok újraaktiválása hello mobilalkalmazás.

## <a name="next-steps"></a>Következő lépések

[Hello multi-factor Authentication SDK letöltése](multi-factor-authentication-sdk.md)

[A Multi-Factor Authentication beállításainak konfigurálása](multi-factor-authentication-whats-next.md)
