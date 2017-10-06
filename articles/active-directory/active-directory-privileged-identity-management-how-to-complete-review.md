---
title: "aaaHow toocomplete egy áttekintése |} Microsoft Docs"
description: "Miután az Azure AD Privileged Identity Management indította egy áttekintése, ismerje meg, hogyan toocomplete, és tekintse meg a hello eredmények"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: f99ddf3ebcf80b51110326064d584f33d8e1679a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocomplete-an-access-review-in-azure-ad-privileged-identity-management"></a>Hogyan toocomplete hozzáférés tekintse át az Azure AD Privileged Identity Management
Kiemelt szerepkörű rendszergazda egyszer emelt szintű hozzáférés is tekintheti a [biztonsági felülvizsgálat elindult](active-directory-privileged-identity-management-how-to-start-security-review.md). Az Azure AD Privileged Identity Management (PIM) küld a felhasználók tooreview hozzáférésüket értesítése e-mailt. Ha a felhasználó nem kapott e-mailt, elküldheti azokat hello utasításokat [hogyan tooperform biztonsági tekintse át](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Után hello biztonsági felülvizsgálat időszak alatt, vagy minden hello felhasználó végzett, az önálló tekintse át, kövesse a cikk toomanage hello felülvizsgálat hello, és hello eredményeket.

## <a name="manage-security-reviews"></a>Biztonsági felülvizsgálat kezelése
1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com/) és select hello **Azure AD Privileged Identity Management** alkalmazás az irányítópulton.
2. Jelölje be hello **értékelést hozzáférési** hello irányítópult szakasza.
3. Válassza ki a megjeleníteni kívánt toomanage hello áttekintése.

Hello hozzáférés felülvizsgálati részletei panel lehetőség áll rendelkezésre egy szám felülvizsgálat kezeléséhez.

![PIM hozzáférés felülvizsgálati gombok – képernyőkép][1]

### <a name="remind"></a>Emlékeztesse
Ha egy áttekintése be van állítva, hogy önmagukat hello felhasználók tekintse át, hello **emlékeztetése** gomb értesítést küld. 

### <a name="stop"></a>Leállítás
Hozzáférés az összes értékelést rendelkeznek befejező dátumát, de használhatja hello **leállítása** gomb toofinish korai azt. Bármely felhasználó még nem ellenőrzött időpontig, ha azok nem lesznek képesek tooafter hello felülvizsgálati le. Nem indítható újra áttekintése után a rendszer leállt.

### <a name="apply"></a>Jelentkezés
Egy áttekintése után, vagy mert hello záró dátum érhető el, vagy manuálisan, megszakította hello **alkalmaz** gomb megvalósítja hello hello felülvizsgálati eredményeit. Ha a felhasználó hozzáférése megtagadva hello felülvizsgálat alatt, akkor hello egy lépést, amely eltávolítja a szerepkör-hozzárendelés.  

### <a name="export"></a>Exportálás
Ha manuálisan tooapply hello eredményét hello biztonsági felülvizsgálat, exportálhatja hello áttekintése. Hello **exportálása** gomb indul el egy CSV-fájl letöltése. Hello elmulasztása az Excel vagy más programok, nyissa meg a CSV-fájlok is kezelheti.

### <a name="delete"></a>Törlés
Ha nem szeretné a hello további átnézni, törölje azt. Hello **törlése** gomb hello felülvizsgálati eltávolítása hello PIM alkalmazást.

> [!IMPORTANT]
> Akkor lesz nem megjelenik egy figyelmeztetés, még törlése előtt, ezért toodelete, tekintse át kívánja. 

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
