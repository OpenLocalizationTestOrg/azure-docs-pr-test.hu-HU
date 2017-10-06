---
title: "aaaProvisioning alkalmazások helyezése hatókörszűrőkkel |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hatókörének szűrők automatizált felhasználókiépítése a ténylegesen létre Ha az objektum nem elégíti ki az üzleti igényeknek támogató alkalmazások tooprevent objektumokat."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0299390dc3fdb70aa9d271e835069a08827d635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Alkalmazások Attribútumalapú üzembe helyezése hatókörszűrőkkel
hello ebben a szakaszban célja tooexplain hogyan toouse hatókörének szűrők toodefine Attribútumalapú szabályok, amelyek meghatározzák, hogy mely felhasználók vannak kiépítve toohello alkalmazás.

## <a name="clauses-and-scope-groups"></a>Feltételek és hatókör
![Hatókör-beállítási szűrője][1] 

Hatókörként szűrők határozzák meg egy vagy több **csoportok hatókör**, minden egyes, amely tartalmaz egy vagy több **záradékok**. egy adott hatókör csoport toosee hello záradékok hello toohello balra nyíl hello csoportnév kattintva bontsa ki.

A **záradék** határozza meg, hogy mely felhasználók vannak toopass hatókör-beállítási szűrője minden felhasználói attribútumok kiértékelésével hello keresztül. Például lehetséges, hogy egy feltételt, amely megköveteli, hogy a felhasználó a "state" attribútum egyenlő New York közt, így csak a győri felhasználók kiépített hello alkalmazásba.

![Hatókör neve][2] 

Minden egyes **hatókör csoport** kezdődik, egy kötelező **záradék**, a fenti hello képernyőfelvételen látható módon. Ehhez a záradékhoz egyszerűen szerint hello felhasználó először meg kell adni toohello alkalmazás által a tartalmazó szűrők értékeléséhez. Ehhez a záradékhoz nem törölték és nem módosítható.

Hozzáadhat új kikötéseket vagy új hatókört csoportok hello megfelelő gomb lenyomásával. Minden hatókör csoport egy nevet adhat szerkesztésével a **hatókör csoportnév** tulajdonság.

## <a name="how-scoping-filters-are-evaluated"></a>Hogyan hatókörének szűrők kiértékelése
Telepítése során, teszteljük minden hozzárendelt felhasználó a tartalmazó szűrők toodetermine szemben ha, hogy a felhasználó érdemel access toohello alkalmazást. Tulajdonképpen minden záradékot, hogy egy tesztet, amely ahhoz, hogy hello felhasználói tooavoid első kiszűri kell átadni. 

Ha több hatókör csoportok definiálva van, minden felhasználóhoz meg kell felelnie legalább egyik tooaccess hello alkalmazás. Minden hatókör csoporton belül azonban hello felhasználónak meg kell felelnie minden záradék toopass adott hatókör csoporthoz. 

Más szóval az eltolásokat tekintheti, hogy a hatókört csoportok volna együtt, illetve, hogy bennük hello záradékok tulajdonképpen és volna együtt. Vegye figyelembe például a hatókör-beállítási szűrője az alábbi hello:

![Hatókör neve][3]  

Hatókör-beállítási szűrője toothis szerint, felhasználók meg kell felelnie a következő hello feltételek, a kiépített toobe:

1. Ezek hozzá kell rendelni toohello alkalmazás.
2. Hello mérnöki részleg kell működnek
3. Munkahelyi kell lenniük a San Francisco vagy Kanada.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Felhasználói kiépítés és megszüntetés tooSaaS alkalmazások automatizálása](active-directory-saas-app-provisioning.md)
* [A felhasználók átadása attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Alkalmazás-kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
* [SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával](active-directory-scim-provisioning.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
