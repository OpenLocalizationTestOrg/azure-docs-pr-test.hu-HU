---
title: "jelentések aaaSign tevékenységet hello Azure Active Directory portálon |} Microsoft Docs"
description: "Bevezetés toosign tevékenységet jelentések hello Azure Active Directory portálon"
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
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 49590d625a08d7dc189a629b89bab2261c2b4780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-reports-in-hello-azure-active-directory-portal"></a>Bejelentkezési tevékenység jelentések hello Azure Active Directory portálon

Az Azure Active Directory (Azure AD) jelentéskészítés hello [Azure-portálon](https://portal.azure.com), hogyan működik a környezet toodetermine szükséges hello információkat kaphat.

a következő összetevők hello architektúra az Azure Active Directory reporting hello foglalja magában:

- **Tevékenység** 
    - **Bejelentkezési tevékenységek** – kezelt alkalmazások és a felhasználói bejelentkezési tevékenységek hello használatával kapcsolatos információért
    - **Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan.
- **Biztonság** 
    - **Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója. További részletek: Kockázatos bejelentkezések.
    - **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. További részletek: Kockázatosként megjelölt felhasználók.

Ez a témakör áttekintést nyújt a hello bejelentkezési tevékenységek.

## <a name="pre-requisite"></a>Előfeltétel

### <a name="who-can-access-hello-data"></a>Hello adatok hozzáférő felhasználók?
* Hello biztonsági rendszergazda vagy a biztonsági olvasó szerepet betöltő felhasználók
* A globális rendszergazdák
* Bármely (nem rendszergazda jogosultságú) felhasználó hozzáfér a saját bejelentkezéseihez 

### <a name="what-azure-ad-license-do-you-need-tooaccess-sign-in-activity"></a>Milyen az Azure AD licencet kell tooaccess bejelentkezési tevékenységet?
* A bérlő rendelkeznie kell egy hozzá társított Azure AD Premium licenc toosee hello akár az összes bejelentkezési tevékenység jelentés


## <a name="signs-in-activities"></a>Bejelentkezési tevékenységek

Felhasználói bejelentkezési jelentés hello hello információk megtalálhatja válaszok tooquestions többek között:

* Mi az a hello bejelentkezési minta egy olyan felhasználó?
* Hány felhasználó jelentkezett be egy adott héten?
* Mi az a bejelentkezéseket hello állapotának?

Az első belépési pont tooall bejelentkezési tevékenységek adatok **bejelentkezések** hello tevékenység részében **Azure Active**.


![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")


Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:

- hello kapcsolódó felhasználói
- hello alkalmazás hello felhasználó van bejelentkezve a
- hello bejelentkezési állapot
- hello bejelentkezési idő

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")

Testre szabhatja hello listanézet kattintva **oszlopok** hello eszköztáron.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")

Ez lehetővé teszi a toodisplay további mezőket, vagy távolítsa el a mezőket, amelyeknek már jelennek meg.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")

Hello listanézet elemére kattintva kap az összes rendelkezésre álló részleteit.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")


## <a name="filtering-sign-in-activities"></a>A bejelentkezési tevékenységek szűrése

lefelé hello toonarrow jelentette, hogy az Ön működik, végezhet hello bejelentkezések adatokat, és a következő mezők hello tooa szintnek:

- Időintervallum
- Felhasználó
- Alkalmazás
- Ügyfél
- Bejelentkezési állapot

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")


Hello **alatt az időtartam alatt** szűrő lehetővé teszi, hogy tooyou toodefine a hello időkereteket adatokat adott vissza.  
Lehetséges értékek:

- 1 hónap
- 7 nap
- 24 óra
- Egyéni

Egyéni időkeret kiválasztásakor beállíthatja a kezdő és a záró időpontot.

Hello **felhasználói** szűrő lehetővé teszi a toospecify hello nevét vagy hello egyszerű felhasználónév (UPN) az Ön számára legfontosabb hello felhasználó.

Hello **alkalmazás** szűrő lehetővé teszi az Ön számára legfontosabb hello alkalmazás toospecify hello nevét.

Hello **ügyfél** szűrő lehetővé teszi az Ön számára legfontosabb hello eszköz toospecify adatai.

Hello **bejelentkezési állapot** szűrő tooselect a következő szűrő hello lehetővé teszi:

- Összes
- Sikeres
- Hiba


## <a name="sign-in-activities-shortcuts"></a>Bejelentkezési tevékenységek parancsikonjai

Ezenkívül tooAzure Active Directory, hello Azure-portálon biztosít két további belépési pontok toosign a tevékenységek adatok:

- Felhasználók és csoportok
- Vállalati alkalmazások


### <a name="users-and-groups-sign-ins-activities"></a>Felhasználók és csoportok bejelentkezési tevékenységei

Felhasználói bejelentkezési jelentés hello hello információk megtalálhatja válaszok tooquestions többek között:

- Mi az a hello bejelentkezési minta egy olyan felhasználó?
- Hány felhasználó jelentkezett be egy adott héten?
- Mi az a bejelentkezéseket hello állapotának?



A belépési pont toothis adatok hello felhasználói bejelentkezési diagram hello **áttekintése** szakaszában **felhasználók és csoportok**.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")

hello felhasználói bejelentkezési grafikon azt ábrázolja, bejelentkezési heti összesítéseinek modulok az összes felhasználó számára egy adott időszakban. hello hello időszak alapértelmezés 30 nap.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")

Egy napon hello bejelentkezési Graph kattintáskor hello bejelentkezési tevékenységek részletes listájának lekérése a nap.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")

Minden egyes sorára hello bejelentkezési tevékenységek listája ad meg hello kijelölt hello bejelentkezhet például részletes információkat:

* Ki jelentkezett be?
* Mi volt hello kapcsolódó UPN?
* Milyen alkalmazás hello célja hello bejelentkezés volt?
* Mi az a hello IP-címe hello bejelentkezés?
* Mi volt a hello állapotának hello bejelentkezés?

Hello **bejelentkezések** beállítás lehetővé teszi az összes felhasználói bejelentkezések teljes körű áttekintést.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")



## <a name="usage-of-managed-applications"></a>Felügyelt alkalmazások használati adatai

A bejelentkezési információk alkalmazás-központú nézetével az alábbi kérdésekre kaphat választ:

* Ki használja az alkalmazásaimat?
* Mik azok a hello legfontosabb 3 alkalmazást a szervezetében?
* Nemrégiben új alkalmazást adtam ki. Mennyire sikeres?

A belépési pont toothis adatok hello legfontosabb 3 alkalmazást a szervezeten belül hello utolsó 30 nap jelentést a hello **áttekintése** szakaszában **vállalati alkalmazások**.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")

hello alkalmazás használati diagramon heti összesítéseinek bejelentkezések az első 3 alkalmazások egy adott időszakban. hello hello időszak alapértelmezés 30 nap.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")

Ha szeretné, amelyen beállíthatja hello fókusz egy adott alkalmazás.


![Jelentéskészítés](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Jelentéskészítés")

Gombra a hello alkalmazás használati diagramot egy napon kapott hello bejelentkezési tevékenységek részletes listáját.


![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")


Hello **bejelentkezések** lehetőséget ad bejelentkezési események tooyour alkalmazások teljes áttekintése.

![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")



## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy a bejelentkezési tevékenység hibakódokkal kapcsolatban további tooknow, tekintse meg a hello [bejelentkezési tevékenység jelentés hibakódok hello Azure Active Directory portálon](active-directory-reporting-activity-sign-ins-errors.md).

