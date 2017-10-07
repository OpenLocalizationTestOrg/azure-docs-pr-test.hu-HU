---
title: "a jelentés hibakódok aaaSign tevékenységet hello Azure Active Directory portálon |} Microsoft Docs"
description: "Bejelentkezési tevékenységekre vonatkozó jelentések hibakódjainak referenciája."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: a0ca5b706bfeb0c7ce669712468a083a394712b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-report-error-codes-in-hello-azure-active-directory-portal"></a>Bejelentkezési tevékenység jelentés hibakódok hello Azure Active Directory portálon

Hello felhasználói bejelentkezések jelentés hello információk megtalálhatja válaszok tooquestions többek között:

- Ki jelentkezett be az Azure Active Directory használatával?
- Melyik alkalmazásba jelentkeztek be?
- Melyik bejelentkezés volt sikertelen, és miért?

Ez a témakör a listák hello hiba-kód és hello kapcsolódó leírása. 

## <a name="how-can-i-display-failed-sign-ins"></a>Hogyan tudom megjeleníteni a sikertelen bejelentkezéseket? 

Az első belépési pont tooall bejelentkezési tevékenységek adatok  **[bejelentkezések](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)**  a hello **tevékenység** szakasza **Azure Active**.


![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Sign-in activity")


A bejelentkezési jelentésben megjelenítheti az összes sikertelen bejelentkezést. Ehhez válassza a **Sikertelen** elemet a **Bejelentkezési állapot** mezőben.


![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Sign-in activity")

Megjelenített hello lista egy elemére kattint, megnyílik a hello **tevékenység részletei: bejelentkezések** panelen. Ez a nézet biztosít, amely Azure Active Directory nyomon követi az bejelentkezéseket, beleértve a hello kapcsolatos összes hello adatokkal **bejelentkezési hibakód** és egy **a hiba oka**.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Sign-in activity")


Alternatív toousing hello Azure portál tooaccess hello bejelentkezések adatokat is használhatja hello [reporting API-val](active-directory-reporting-api-getting-started-azure-portal.md).


hello következő szakasz azt ismerteti az összes lehetséges hibákat és hello teljes áttekintése kapcsolódó leírása. 

## <a name="error-codes"></a>Hibakódok

| Hiba| Leírás |
| --- | --- |
| 50001| hello szolgáltatás egyszerű X nevű Y nevű hello bérlő nem található. Ez akkor fordulhat elő, ha hello alkalmazás nincs telepítve hello rendszergazda hello bérlő. Vagy az erőforrás egyszerű hello könyvtár nem található vagy érvénytelen|
| 50008| SAML-előfeltétel hiányzik vagy helytelenül konfigurált hello jogkivonat.|
| 50011| hello válaszcímet hiányzik, nincs megfelelően konfigurálva, vagy nem felel meg a válasz címek hello alkalmazáshoz konfigurált.|
| 50053| Fiókja zárolva van, mert a felhasználó próbált toosign a túl sokszor utasította az egy helytelen felhasználói azonosító vagy jelszó.|
| 50054| Régi jelszót használt a hitelesítéshez.|
| 50055| Érvénytelen jelszó, lejárt jelszót írt be.|
| 50057| A felhasználói fiók le van tiltva.|
| 50058| Nincs információ a felhasználó identitása között található a megadott hitelesítő adatok vagy a felhasználó nem található bérlő vagy csendes bejelentkezési kérelmet küldött, de nem a felhasználó bejelentkezett vagy szolgáltatást, ez azonban nem tooauthenticate hello felhasználói.|
| 50074| Erős (kéttényezős) hitelesítésre van szükség|
| 50079| Felhasználói tooenroll igényeihez kéttényezős hitelesítést|
| 50126| Érvénytelen felhasználónév vagy jelszó, vagy érvénytelen helyszíni felhasználónév vagy jelszó.|
| 50131| Különböző feltételes hozzáférési hibákban használatos. Például rossz Windows-eszköz állapotba kerül, a kérelem blokkolva toosuspicious tevékenység, a hozzáférési házirend és a biztonsági házirend miatt döntéseket.|
| 50133| Munkamenet tooexpiration vagy a legutóbbi jelszó módosítása miatt érvénytelen.|
| 50144| A felhasználó Active Directory jelszava lejárt.|
| 65001| Alkalmazás X nem rendelkezik engedéllyel tooaccess alkalmazás Y vagy hello engedély vissza lett vonva. Vagy hello felhasználó vagy a rendszergazda nem hozzájárult toouse hello alkalmazás azonosítója X. küldése egy interaktív engedélyezési kérelem a felhasználó-és erőforrás. Vagy hello felhasználó vagy a rendszergazda nem hozzájárult nevében hello App ID X. küldése egy engedélyezési kérelem tooyour Bérlői rendszergazda tooact toouse hello alkalmazás: Y erőforrás: z-ig.|
| 65005| hello alkalmazás erőforrás-hozzáférési lista nem tartalmaz felderíthető alkalmazások hello erőforrás vagy hello ügyfélalkalmazás kért hozzáférés tooresource, amely nem volt megadva a szükséges erőforrás-hozzáférési lista vagy visszaadott rossz Graph-szolgáltatás a szükséges kérelem vagy az erőforrás nem található.|
| 70001| hello alkalmazást X Y nevű hello bérlő nem található. Ez is hiba akkor fordulhat elő ha hello alkalmazás nincs telepítve által hello hello bérlői vagy hozzájárult tooby rendszergazdája minden olyan felhasználó hello bérlőben. Előfordulhat, hogy a hitelesítési kérelem toohello megfelelő bérlő elküldött.|
| 80001| Nem érhető el hitelesítési ügynök.|
| 80002| A hitelesítési ügynök jelszó-érvényesítési kérése túllépte az időkorlátot.|
| 80003| A hitelesítési ügynök érvénytelen választ kapott.|
| 80004| A bejelentkezési kérésben helytelen egyszerű felhasználónevet (UPN-t) használtak.|
| 80005| Hitelesítési ügynök: hiba történt.|
| 80007| Hitelesítési ügynök nem tooconnect tooActive könyvtár.|
| 80010| Hitelesítési ügynök nem toodecrypt jelszót.|
| 81001| A felhasználó Kerberos-jegye túl nagy.|
| 81002| Nem lehet toovalidate felhasználói Kerberos jegy.|
| 81003| Nem lehet toovalidate felhasználói Kerberos jegy.|
| 81004| A Kerberos-hitelesítési kísérlet meghiúsult.|
| 81008| Nem lehet toovalidate felhasználói Kerberos jegy.|
| 81009| Nem lehet toovalidate felhasználói Kerberos jegy.|
| 81010| Zökkenőmentes egyszeri bejelentkezés sikertelen volt, mert hello felhasználói Kerberos jegy lejárt vagy érvénytelen.|
| 81011| Nem lehet toofind felhasználói objektum hello felhasználói Kerberos jegy szereplő információ alapján.|
| 81012| a tooAzure AD toosign próbált hello felhasználói eltér hello felhasználói hello eszközre.|
| 81013| Nem lehet toofind felhasználói objektum hello felhasználói Kerberos jegy szereplő információ alapján.|
| 90014| Egyes esetekben használják, ha egy várt mező nincs jelen a hello hitelesítő adatot.|
| 90093| A grafikon tiltott hibakód hello kérelem lett visszaadva.|



## <a name="next-steps"></a>Következő lépések

További részletekért lásd: hello [bejelentkezési Tevékenységjelentések hello Azure Active Directory portálon](active-directory-reporting-activity-sign-ins.md).

