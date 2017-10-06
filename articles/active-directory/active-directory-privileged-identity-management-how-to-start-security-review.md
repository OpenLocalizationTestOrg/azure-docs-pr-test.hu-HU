---
title: "aaaHow toostart egy áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate hozzáférés tekintse át a kiemelt identitásokat hello Azure Privileged Identity Management alkalmazás."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 24feac307f77c69b5d68d6ae0623dbcb52416b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostart-an-access-review-in-azure-ad-privileged-identity-management"></a>Hogyan toostart hozzáférés tekintse át az Azure AD Privileged Identity Management
Szerepkör-hozzárendelések "elavult" válnak, amikor a felhasználók privilegizált hozzáférést, amelyek többé nem kell. Rendelés tooreduce hello kockázatának ezek elavult szerepkör-hozzárendelések, a kiemelt szerepkörű rendszergazda felhasználók kapott hello szerepkörtől rendszeresen tekintse át. Ez a dokumentum egy hozzáférés-ellenőrzés indítása az Azure AD Privileged Identity Management (PIM) hello lépéseit ismerteti.

## <a name="start-an-access-review"></a>Hozzáférés-ellenőrzés indítása
> [!NOTE]
> Ha hello PIM alkalmazás tooyour irányítópult hello Azure-portálon még nem adott hozzá, tekintse meg a hello lépéseit [Ismerkedés az Azure Privileged Identity Management szolgáltatással](active-directory-privileged-identity-management-getting-started.md)
> 
> 

Hello PIM alkalmazás fő lapján nincsenek három módon toostart egy áttekintése:

* **Hozzáférési értékelést** > **hozzáadása**
* **Szerepkörök** > **felülvizsgálati** gomb
* SELECT hello adott szerepkör toobe áttekintette hello szerepkörök listából > **felülvizsgálati** gomb

Amikor rákattint az hello **tekintse át** gomb hello **egy hozzáférés-ellenőrzés indítása** panel jelenik meg. A panel éppen folyamatban tooconfigure hello felülvizsgálati néven és határidő, választja a szerepkör tooreview, és döntse el, akik végrehajtják a hello áttekintése.

![Indítsa el az áttekintése – képernyőkép][1]

### <a name="configure-hello-review"></a>Hello felülvizsgálati konfigurálása
toocreate hozzáférés tekintse át, akkor a tooname kell, és azt, és állítsa a kezdő és záró dátumát.

![Konfigurálja a felülvizsgálati – képernyőkép][2]

Ellenőrizze a hello hossza hello áttekintése amíg a felhasználók toocomplete. Ha befejezte a hello záró dátum előtt, mindig le is hello felülvizsgálati korai.

### <a name="choose-a-role-tooreview"></a>Válassza ki a szerepkör tooreview
Minden egyes felülvizsgálati csak egy szerepkör összpontosít. Ha hello áttekintése indította el egy adott szerepkör panel, toochoose szerepkör most lesz szüksége.

1. Keresse meg a túl**tekintse át a szerepköri tagság**
   
    ![Tekintse át a szerepköri tagság – képernyőkép][3]
2. Hello listából válassza ki egy szerepkört.

### <a name="decide-who-will-perform-hello-review"></a>Döntse el, akik végrehajtják a hello áttekintése
Ellenőrzés végrehajtása esetén három lehetőség áll rendelkezésre. Hello felülvizsgálati toosomeone rendelhet más toocomplete, akkor megteheti ezt, vagy beállíthatja, hogy minden felhasználó, tekintse át a saját hozzáférést.

1. Keresse meg a túl**felülvizsgálók kiválasztása**
   
    ![Válassza ki a felülvizsgálók – képernyőkép][4]
2. Hello lehetőségek közül választhat:
   
   * **Válassza ki a felülvizsgáló**: használja ezt a beállítást, ha nem biztos benne, aki hozzá kell férnie. Ezzel a lehetőséggel hello felülvizsgálati tooa erőforrás tulajdonosa vagy a csoport manager toocomplete rendelhet.
   * **A nekem**: hasznos, ha azt szeretné, toopreview hogyan tooreview kívánt nevében nem személyek vagy a hozzáférés ellenőrzi, hogy a munkahelyi.
   * **Tagok áttekintése maguk**: használja ezt a beállítást toohave hello felhasználók tekintse át a saját szerepkör-hozzárendelések.

### <a name="start-hello-review"></a>Hello felülvizsgálat indítása
Végül ott hello beállítás toorequire, hogy a felhasználók ha azok elfogadják a hozzáférését meg okot. Ha szeretné adjon meg egy leírást, hello nézze át, és válassza ki **Start**.

Győződjön meg arról, hogy engedélyezi a felhasználóknak, hogy egy entitás áttekintése, és a megjelenítésükhöz [hogyan tooperform hozzáférés tekintse át](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-hello-access-review"></a>Hello áttekintése kezelése
Előrehaladásának hello módon hello felülvizsgálók végezze el az Azure AD PIM irányítópult hello áttekintette, a hello hozzáférési szakasz ellenőrzi. Nincs hozzáférési jogosultsága amíg hello címtárban módosulnak [hello felülvizsgálati befejezése](active-directory-privileged-identity-management-how-to-complete-review.md).

Hello felülvizsgálati időszak felett van, akkor is emlékeztesse felhasználók toocomplete felülvizsgálatához, vagy ha stop hello felülvizsgálati korai hello hozzáférés ellenőrzi, hogy a szakasz.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="pim-table-of-contents"></a>A PIM tartalomjegyzék
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
