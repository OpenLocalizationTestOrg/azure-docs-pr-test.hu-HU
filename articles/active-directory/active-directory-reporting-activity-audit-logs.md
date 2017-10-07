---
title: "aaaAudit tevékenység jelentések hello Azure Active Directory portálon |} Microsoft Docs"
description: "Bevezetés toohello naplózási Tevékenységjelentések hello Azure Active Directory portálon"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 1567673f5030fc707b017c069f2ba7587962e5cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="audit-activity-reports-in-hello-azure-active-directory-portal"></a>Naplózási Tevékenységjelentések hello Azure Active Directory portálon 

A jelentéskészítés az Azure Active Directory (Azure AD), toodetermine hogyan működik a környezetében szükséges hello információkat kaphat.

a következő összetevők hello architektúra az Azure AD reporting hello foglalja magában:

- **Tevékenység** 
    - **Bejelentkezési tevékenységek** – kezelt alkalmazások és a felhasználói bejelentkezési tevékenységek hello használatával kapcsolatos információért
    - **Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan.
- **Biztonság** 
    - **Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója. További részletek: Kockázatos bejelentkezések.
    - **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. További részletek: Kockázatosként megjelölt felhasználók.

Ez a témakör áttekintést nyújt a hello naplózási tevékenységek.
 
## <a name="who-can-access-hello-data"></a>Hello adatok hozzáférő felhasználók?
* Hello biztonsági rendszergazda vagy a biztonsági olvasó szerepet betöltő felhasználók
* A globális rendszergazdák
* Az egyedi (nem rendszergazda jogosultságú) felhasználók csak a saját tevékenységüket láthatják


## <a name="audit-logs"></a>Naplók

hello naplók az Azure Active Directoryban adja meg a rendszer tevékenységek megfelelőségi rögzíti.  
Az első belépési pont tooall adatok naplózása van **naplók** a hello **tevékenység** szakasza **Azure Active Directory**.

![Naplók](./media/active-directory-reporting-activity-audit-logs/61.png "Naplók")

Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:

- hello dátum és idő hello előfordulás
- hello kezdeményező / aktor (*ki*) tevékenység 
- tevékenység hello (*mi*) 
- hello cél

![Naplók](./media/active-directory-reporting-activity-audit-logs/18.png "Naplók")

Testre szabhatja hello listanézet kattintva **oszlopok** hello eszköztáron.

![Naplók](./media/active-directory-reporting-activity-audit-logs/19.png "Naplók")

Ez lehetővé teszi a toodisplay további mezőket, vagy távolítsa el a mezőket, amelyeknek már jelennek meg.

![Naplók](./media/active-directory-reporting-activity-audit-logs/21.png "Naplók")


Hello listanézet elemére kattintva kap az összes rendelkezésre álló részleteit.

![Naplók](./media/active-directory-reporting-activity-audit-logs/22.png "Naplók")


## <a name="filtering-audit-logs"></a>Auditnaplók szűrése

lefelé hello toonarrow jelentette, hogy az Ön működik, végezhet hello naplózási adatok használatával a következő mezők hello tooa szintnek:

- Dátumtartomány
- Kezdeményező (Szereplő)
- Kategória
- Tevékenység erőforrástípusa
- Tevékenység

![Naplók](./media/active-directory-reporting-activity-audit-logs/23.png "Naplók")


Hello **dátumtartományának** szűrő lehetővé teszi, hogy tooyou toodefine a hello időkereteket adatokat adott vissza.  
Lehetséges értékek:

- 1 hónap
- 7 nap
- 24 óra
- Egyéni

Egyéni időkeret kiválasztásakor beállíthatja a kezdő és a záró időpontot.

Hello **által kezdeményezett** szűrő lehetővé teszi, hogy Ön toodefine egy szereplő nevét vagy annak univerzális egyszerű felhasználónév (UPN).

Hello **kategória** szűrő tooselect a következő szűrő hello lehetővé teszi:

- Összes
- Alapvető kategória
- Alapvető könyvtár
- Önkiszolgáló jelszókezelés
- Önkiszolgáló csoportkezelés
- Fiók kiépítése – Automatizált jelszóváltás
- Meghívott felhasználók
- MIM szolgáltatás
- Identity Protection
- B2C

Hello **tevékenység erőforrástípus** szűrő lehetővé teszi tooselect hello következő szűrése:

- Összes 
- Csoport
- Címtár
- Felhasználó
- Alkalmazás
- Szabályzat
- Eszköz
- Egyéb

Ha bejelöli **csoport** , **tevékenység erőforrástípus**, tooalso kap egy kiegészítő szűrőt kategóriát, amely lehetővé teszi, adjon meg egy **forrás**:

- Azure AD
- O365


Hello **tevékenység** szűrő hello kategória és tevékenység erőforrás típusa választott ki alapul. Kiválaszthatja, hogy egy adott tevékenységre, válassza ki az összes vagy toosee szeretné. 

Kaphat az összes naplózási tevékenység hello listája tenantdomain/tevékenységek/auditActivityTypes hello Graph API https://graph.windows.net/$ használatával? api-version = beta, ahol $tenantdomain = a tartomány nevét, vagy tekintse meg a toohello cikk [ellenőrzési jelentés események](active-directory-reporting-audit-events.md).


## <a name="audit-logs-shortcuts"></a>Rövidebb utak a naplók eléréséhez

Ezenkívül túl**Azure Active Directory**, hello Azure-portál lehetővé teszi két további belépési pontok tooaudit adatokat:

- Felhasználók és csoportok
- Vállalati alkalmazások

### <a name="users-and-groups-audit-logs"></a>Felhasználók és csoportok auditnaplói

A felhasználó és csoport alapuló naplózási jelentések kaphat a válaszok tooquestions többek között:

- Milyen típusú frissítések lettek alkalmazott hello felhasználók?

- Hány felhasználó lett módosítva?

- Hány jelszó lett módosítva?

- Mit csinált a rendszergazda egy adott címtárban?

- Mik azok a hozzáadott hello csoportok?

- Történt-e tagsági változás valamelyik csoportban?

- Megváltoztatta a csoport tulajdonosainak hello?

- Milyen licencek tooa csoport vagy felhasználó hozzárendelt?

Ha csak naplózási adatokat, amelyek kapcsolódó toousers és csoportok tooreview, egy szűrt nézet alatt található **naplók** a hello **tevékenység** hello szakasza **felhasználók és csoportok**. Ennél a lehetőségnél a **Felhasználók és csoportok** van előre kiválasztva **tevékenység-erőforrástípusként**.

![Naplók](./media/active-directory-reporting-activity-audit-logs/93.png "Naplók")

### <a name="enterprise-applications-audit-logs"></a>Vállalati alkalmazások naplói

Az alkalmazás-alapú naplózási jelentések, akkor létrehozhat válaszok tooquestions többek között:

* Mik azok a hozzáadott vagy frissített hello alkalmazások?
* Mik azok a hello alkalmazások, amelyek el lettek távolítva?
* Megváltozott valamelyik alkalmazás egyszerű szolgáltatásneve?
* Megváltoztatta a kérelmek hello nevek?
* Hozzájárulás tooan alkalmazás számára megadott?

Ha csak naplózási adatokat, amelyek kapcsolódó tooyour alkalmazások tooreview, egy szűrt nézet alatt található **naplók** a hello **tevékenység** hello szakasza **vállalati alkalmazások**  panelen. Ennél a lehetőségnél a **Vállalati alkalmazások** van előre kiválasztva **tevékenység-erőforrástípusként**.

![Naplók](./media/active-directory-reporting-activity-audit-logs/134.png "Naplók")

Szűrheti a nézet további le toojust **csoportok** vagy csak **felhasználók**.

![Naplók](./media/active-directory-reporting-activity-audit-logs/25.png "Naplók")


## <a name="next-steps"></a>Következő lépések

Jelentéskészítési áttekintését lásd: hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).

