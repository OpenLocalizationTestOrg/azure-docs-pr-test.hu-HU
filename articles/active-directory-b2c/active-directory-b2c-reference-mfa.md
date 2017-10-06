---
title: "Az Azure Active Directory B2C: Többtényezős hitelesítés |} Microsoft Docs"
description: "Hogyan tooenable felhasználók felé néző alkalmazásokban a multi-factor Authentication által védett Azure Active Directory B2C-vel"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 29beb1e6ab5d8ab5a65f9c5c068d9e71c60418dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Az Azure Active Directory B2C: Többtényezős hitelesítés engedélyezése a felhasználók felé néző alkalmazásokban
Az Azure Active Directory (Azure AD) B2C integrálható közvetlenül [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) , hogy a felhasználók felé néző alkalmazásokban biztonsági toosign felfelé és a bejelentkezési élmény második réteget is hozzáadhat. És ehhez egyetlen kódsort írása nélkül. Jelenleg támogatott telefonhívási és az SMS-ellenőrzéseket. Ha már létrehozott regisztráció és bejelentkezés házirendek, a multi-factor Authentication továbbra is engedélyezheti.

> [!NOTE]
> Többtényezős hitelesítés is engedélyezhető, nem csak a meglévő házirendek szerkesztésével regisztráció és bejelentkezés házirendek létrehozásakor.
> 
> 

Ez a szolgáltatás segít az alkalmazások, például hello következő helyzetek kezelésére:

* A multi-factor Authentication tooaccess egy alkalmazás nem feltétlenül szükséges, de tooaccess van szükség egy másikat. Például hello fogyasztói alkalmazásba automatikus biztosítási közösségi vagy helyi fiókkal jelentkezhet, de előtt ellenőriznie kell hello telefonszám hello otthoni biztosítási alkalmazás elérésének regisztrált hello azonos könyvtárban.
* A multi-factor Authentication tooaccess alkalmazás általában nem igényelnek, de tooaccess hello bizalmas részei belül van szükség. Például hello fogyasztói tooa banki alkalmazás ellenőrizze számlaegyenlegeit és közösségi vagy helyi fiók tud bejelentkezni, de a vezetékes átvitel előtt hello telefonszám ellenőriznie kell.

## <a name="modify-your-sign-up-policy-tooenable-multi-factor-authentication"></a>A regisztrációs szabályzatban tooenable multi-factor Authentication módosítása
1. [Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **Regisztrálási szabályzatok** lehetőségre.
3. Kattintson a regisztrációs szabályzatban (például "B2C_1_SiUp") tooopen azt.
4. Kattintson a **a multi-factor authentication** hello tehetik **állapot** túl**ON**. Kattintson az **OK** gombra.
5. Kattintson a **mentése** hello panel hello tetején.

Hello házirend tooverify hello végfelhasználói élmény hello "Futtatás most" funkciót is használja. Erősítse meg a következő hello:

A felhasználói fiók jön létre a címtárban még hello multi-factor Authentication lépés előtt. Hello lépés során hello fogyasztói tooprovide rendszerbe telefonszám, és győződjön meg arról, hogy kapcsolatba. Ha az ellenőrzés sikeres, a hello telefonszám csatlakozik-e toohello végfelhasználói fiók későbbi használatra. Akkor is, ha hello fogyasztói megszakítja, vagy elutasítja azokat a, általa is megkéri tooverify telefonszám újra során hello következő bejelentkezéskor a (a többtényezős hitelesítés engedélyezve).

## <a name="modify-your-sign-in-policy-tooenable-multi-factor-authentication"></a>A bejelentkezési házirend tooenable multi-factor Authentication módosítása
1. [Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kattintson a **bejelentkezési házirendek**.
3. Kattintson a bejelentkezési házirend (például "B2C_1_SiIn") tooopen azt. Kattintson a **szerkesztése** hello panel hello tetején.
4. Kattintson a **a multi-factor authentication** hello tehetik **állapot** túl**ON**. Kattintson az **OK** gombra.
5. Kattintson a **mentése** hello panel hello tetején.

Hello házirend tooverify hello végfelhasználói élmény hello "Futtatás most" funkciót is használja. Erősítse meg a következő hello:

Amikor hello fogyasztói (fiók használatával jelentkezik be egy közösségi vagy helyi), ha csatlakozik egy ellenőrzött és érvényes telefonszámot toohello végfelhasználói fiók, akkor kapcsolatba kell adnia tooverify azt. A telefonszámmal nem kapcsolódik, ha a hello fogyasztói egy tooprovide kapcsolatba, és azt. A sikeres ellenőrzés hello telefonszám csatolt toohello végfelhasználói fiók későbbi használatra.

## <a name="multi-factor-authentication-on-other-policies"></a>Egyéb házirendek többtényezős hitelesítést
Regisztráció és bejelentkezés házirendek a fenti leírtak is lehetséges tooenable többtényezős hitelesítést a regisztrációs vagy bejelentkezési házirendek és a jelszó alaphelyzetbe állítása a házirendeket. Házirendek Profilszerkesztési hamarosan elérhető lesz.

