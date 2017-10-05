---
title: "A csoportok dinamikus tagság hibaelhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 32947e8cc69c9a48d9a285bf0a37ab3398571f86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Dinamikus csoporttagságok hibaelhárítása
**Egy szabály beállítása a csoport, de nincs tagságát módosul, a csoport**<br/>Ellenőrizze, hogy a **engedélyezése delegált csoportkezelés** beállítása **Igen** a a **konfigurálása** fülre. Ez a beállítás csak akkor, ha Ön felhasználóként van bejelentkezve a felhasználó, akinek hozzá van rendelve egy Azure Active Directory Premium licenc jelenik meg. Ellenőrizze a szabály a felhasználói attribútumok értékeit: vannak-e, amely megfelel a szabálynak felhasználók?

**Egy szabály beállítása, de most a szabály meglévő tagjai törlődnek**<br/>Ez az elvárt működés. A csoport tagjai meglévő törlődnek, amikor a szabály engedélyezve van-e módosítani. A szabály értékelése által visszaadott felhasználók tagjai a csoportba kerülnek.     

**Nem szerepel az tagsága instantly hozzáadása vagy szabály, miért nem módosítása esetén?**<br/>Dedikált gyűjteménytagság értékelése egy aszinkron háttér folyamat rendszeres időközönként történik. Mennyi ideig tart határozza meg a könyvtárban lévő felhasználók számát és méretét a csoport, a szabály eredményeként jött létre. Felhasználók kis mennyiségű mappák általában, megjelenik a csoporttagsági változások legfeljebb néhány perc múlva. A felhasználók sok könyvtárak 30 percet is igénybe vehet, vagy hosszabb adatokkal való feltöltéséhez.

### <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
