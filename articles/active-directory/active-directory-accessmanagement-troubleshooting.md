---
title: "dinamikus csoporttagság aaaTroubleshooting csoportok |} Microsoft Docs"
description: "Dinamikus csoporttagság csoportok tippek az Azure ad-ben."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: d792fc406288844e2c5dbe3702c2c9870d09473e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Dinamikus csoporttagságok hibaelhárítása
**Egy szabály beállítása a csoporton, de nincs tagságát frissített hello csoportban**<br/>Győződjön meg arról, hogy hello **engedélyezése delegált csoportkezelés** beállítás értéke túl**Igen** a hello **konfigurálása** fülre. Ez a beállítás csak akkor, ha nincs bejelentkezve a felhasználó toowhom egy Azure Active Directory Premium licenc rendeli jelenik meg. Ellenőrizze a felhasználói attribútumokat a hello szabály hello értékeket: vannak-e, amely megfelel a hello szabály felhasználók?

**Egy szabály beállítása, de most hello meglévő hello szabály tagjai törlődnek**<br/>Ez az elvárt működés. Hello csoport tagjai meglévő törlődnek, amikor a szabály engedélyezve van-e módosítani. hello szabály értékelése által visszaadott hello felhasználók tagok toohello csoport hozzá szeretné adni.     

**Nem szerepel az tagsága instantly hozzáadása vagy szabály, miért nem módosítása esetén?**<br/>Dedikált gyűjteménytagság értékelése egy aszinkron háttér folyamat rendszeres időközönként történik. Mennyi ideig hello folyamat veszi hello csoport hello szabály eredményeként jött létre a könyvtár és hello méretű felhasználók hello számát határozza meg. Felhasználók kis mennyiségű mappák általában, lásd a hello csoporttagsági változások legfeljebb néhány perc múlva. A felhasználók sok könyvtárak is igénybe vehet, 30 perc vagy hosszabb toopopulate.

### <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
