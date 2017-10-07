---
title: "aaaFind tevékenység jelentései hello Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toofind Azure Active Directory-tevékenység jelentései hello Azure-portálon."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f8d7a968403e10ccc5319f27fedad38b1553ded0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-activity-reports-in-hello-azure-portal"></a>Tevékenységjelentések található hello Azure-portálon

Ha hello Azure klasszikus portál toohello az Azure-portálon, kap egy új tevékenységi naplóit Azure Active Directory (Azure AD) nézze meg. Az egy nemrég végrehajtott [blogbejegyzés](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), azt ismertetik, hogyan láthatja, tevékenység dolgozik a hello Azure-portálon hello erőforrás hello környezetében naplózza. Ez a cikk azt ismerteti hogyan toofind jelent a hello hello Azure-portálon a klasszikus Azure portálon használt.

## <a name="whats-new"></a>Újdonságok

A klasszikus Azure portálon hello jelentések kategóriába oszthatók:

1.  Biztonsági jelentések
2.  Tevékenységjelentések
3.  Integrált alkalmazás jelentések

### <a name="activity-and-integrated-app-reports"></a>Tevékenység és integrált alkalmazás jelentések

Környezetfüggő jelentéseihez hello Azure-portálon, a meglévő jelentések egyesülnek egyetlen nézetben. Egyetlen, a mögöttes API hello adatok toohello nézetét biztosítja.

e nézet, a hello toosee **Azure Active Directory** panel alatt **tevékenység**, jelölje be **naplók**.

![Naplók](./media/active-directory-reporting-migration/482.png "Naplók")

a következő jelentések hello ebben a nézetben van összevonva:

-   Ellenőrzési jelentés
-   Jelszó-visszaállítási tevékenység
-   Jelszó-visszaállítási regisztrációs tevékenység
-   Önkiszolgáló csoportok tevékenységéről
-   Office365 csoport nevének módosítása
-   Tevékenység-kiépítési
-   Jelszó-helyettesítő állapota
-   Alkalmazás-kiépítési hibák


Alkalmazás használati jelentés hello bővült, és szerepel a hello **bejelentkezések** nézet. e nézet, a hello toosee **Azure Active Directory** panel alatt **tevékenység**, jelölje be **bejelentkezések**.

![Bejelentkezések nézet](./media/active-directory-reporting-migration/483.png "bejelentkezések megtekintése")

Hello **bejelentkezések** nézet tartalmazza az összes felhasználói bejelentkezéseket. Használhatja az információk tooget alkalmazás használati adatait. Is megtekintheti az alkalmazásadatok használati hello **vállalati alkalmazások** áttekintése, a hello **kezelése** szakasz.

![Vállalati alkalmazások](./media/active-directory-reporting-migration/484.png "vállalati alkalmazások")

## <a name="access-a-specific-report"></a>Hozzáférés egy adott jelentés

Bár az Azure-portálon hello egyetlen nézetben kínál, is vessen egy pillantást konkrét jelentéseket.

### <a name="audit-logs"></a>Naplók

A válasz toocustomer visszajelzést hello Azure-portálon, használhatja a speciális szűrési tooaccess hello kívánt adatokat is. Használhat egy szűrő egy *tevékenységkategóriákkal*, hello különböző típusú tevékenység sorolja fel, amelyek naplózza az Azure AD-ben. toonarrow eredmények toowhat keres, kiválaszthatja, hogy egy kategóriát.

Például ha érdekli, csak a tevékenységek kapcsolódó tooself-szolgáltatás jelszó-átállításra, választhat hello **önkiszolgáló jelszókezelés** kategóriát. hello kategóriák látja dolgozik hello erőforrás alapulnak.  

![Kategória beállítások hello szűrő naplók lapon](./media/active-directory-reporting-migration/06.png "hello szűrő naplók lapon kategória beállításai")

Tevékenységkategóriák a következők:

- Alapvető könyvtár
- Önkiszolgáló jelszókezelés
- Önkiszolgáló csoportkezelés
- Fiók kiépítése

### <a name="application-usage"></a>Az alkalmazás használatának

az összes olyan alkalmazáshoz, vagy egy önálló alkalmazás, az alkalmazások használatára vonatkozó a részletek tooview **tevékenység**, jelölje be **bejelentkezések**. toonarrow hello eredmény elérése érdekében szűrheti a felhasználó vagy alkalmazás nevét.

![Szűrő bejelentkezési események lapot](./media/active-directory-reporting-migration/07.png "bejelentkezési események szűrése lap")

### <a name="security-reports"></a>Biztonsági jelentések

#### <a name="azure-ad-anomalous-activity-reports"></a>Az Azure AD rendellenes tevékenységet jelentések

Az Azure AD rendellenes tevékenységet biztonsági jelentéseit a klasszikus Azure portálon hello konszolidált tooprovide volt, egy, a központi nézettel. Ebben a nézetben látható az összes biztonsági kockázati eseményekről, hogy az Azure AD képesek észlelni és jelentést.

hello a következő táblázat felsorolja a hello Azure AD rendellenes tevékenységet biztonsági jelentések és a megfelelő kockázat eseménytípusok hello Azure-portálon.

| Az Azure AD rendellenes tevékenységgel kapcsolatos jelentés |  Identity protection kockázati esemény típusa|
| :--- | :--- |
| Felhasználók, akiknek kiszivárogtak a hitelesítő adatai | Kiszivárgott hitelesítő adatok |
| Rendszertelen bejelentkezési tevékenység | Lehetetlen odautazás tooatypical helyek |
| Bejelentkezések potenciálisan fertőzött eszközökről | Bejelentkezések fertőzött eszközökről|
| Bejelentkezések ismeretlen forrásokról | Névtelen IP-címről történő bejelentkezések |
| Bejelentkezések gyanús tevékenységeket mutató IP-címekkel | Bejelentkezések gyanús tevékenységeket mutató IP-címekkel |
| - | Ismeretlen helyekről történt bejelentkezések |

az Azure AD rendellenes tevékenységet biztonsági következő hello jelentések nem tartoznak kockázati események hello Azure-portálon:

* Több hibát követő bejelentkezések
* Bejelentkezések különböző földrajzi régiókból

Ezek a jelentések továbbra is elérhetőek hello a klasszikus Azure portálon, de azok elavulttá válik a jövőbeli hello valamikor.

További információkért tekintse át [Az Azure Active Directory kockázati eseményeivel](active-directory-identity-protection-risk-events.md) foglalkozó cikket.  


#### <a name="detected-risk-events"></a>Észlelt kockázati események

Hello Azure-portálon, Ön hozzáférhet hello észlelt kockázati események kapcsolatos jelentéseket **Azure Active Directory** panel alatt **biztonsági**. A következő jelentések hello észlelt kockázati események követi:   

- Felhasználók veszélyben
- Kockázatos bejelentkezések

![Biztonsági jelentések](./media/active-directory-reporting-migration/04.png "biztonsági jelentések")

Biztonsági jelentésekkel kapcsolatos további információkért lásd:

- [A kockázat biztonsági jelentés hello Azure Active Directory portálon felhasználója](active-directory-reporting-security-user-at-risk.md)
- [Kockázatos bejelentkezések jelentés hello Azure Active Directory portálon](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-hello-azure-classic-portal-vs-hello-azure-portal"></a>Tevékenységjelentések a hello hello Azure-portál és a klasszikus Azure portálon

hello ebben a szakaszban szereplő táblázat hello a klasszikus Azure portálon a meglévő jelentéseken. Azt is bemutatja, hogyan férhetnek hello ugyanazokat az információkat a hello Azure-portálon.

minden tooview adatok, a hello naplózása **Azure Active Directory** panelen, a **tevékenység**, lépjen túl**a naplók**.

![Naplók](./media/active-directory-reporting-migration/61.png "Naplók")

| klasszikus Azure portál                 | az Azure-portálon hello toofind                                                         |
| ---                                  | ---                                                                        |
| Naplók                           | A **Tevékenységkategóriákkal**, jelölje be **Core Directory**.                       |
| Jelszó-visszaállítási tevékenység              | A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**. |
| Jelszó-visszaállítási regisztrációs tevékenység | A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.     |
| Önkiszolgáló csoportok tevékenységéről         | A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló csoportkezelési**.        |
| Tevékenység-kiépítési        | A **Tevékenységkategóriákkal**, jelölje be **fiók a felhasználók átadása**.         |
| Jelszó-helyettesítő állapota             | A **Tevékenységkategóriákkal**, jelölje be **automatikus App jelszó helyettesítő**.      |
| Alkalmazás-kiépítési hibák          | A **Tevékenységkategóriákkal**, jelölje be **fiók a felhasználók átadása**.        |
| Office365 csoport nevének módosítása         | A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**. A **tevékenység erőforrástípus**, jelölje be **csoport**. A **tevékenység forrása**, jelölje be **Office 365-csoportok**.|

tooview hello **Alkalmazáshasználat** jelentésére hello **Azure Active Directory** panel alatt **kezelése**, jelölje be **vállalati alkalmazások**, majd válassza ki **bejelentkezések**.


![Vállalati alkalmazások bejelentkezések jelentés](./media/active-directory-reporting-migration/199.png "vállalati alkalmazások bejelentkezések jelentés")

## <a name="next-steps"></a>Következő lépések

Jelentéskészítési áttekintését lásd: hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).
