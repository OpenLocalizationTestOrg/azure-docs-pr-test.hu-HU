---
title: "adott csoporthoz tartozó felhasználók az Azure Active Directoryban aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate egy csoportot az Azure Active Directoryban és tagok toohello csoport hozzáadása"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc583a7f02ce50e7f3b2c8f97a9c032a3e2dc33a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [klasszikus Azure portál](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Ez a cikk azt ismerteti, hogyan toocreate és feltöltése az Azure Active Directoryban egy új csoportot. Egy csoport tooperform felügyeleti feladatokhoz, mint az hozzárendelése licencek vagy hozzáférési engedélyekkel a felhasználók vagy eszközök tooa száma egyszerre használja.

## <a name="how-do-i-create-a-group"></a>Hogyan hozható létre csoport?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **összes csoport**.

   ![Nyitó hello csoportok panelen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. A hello **felhasználók és csoportok – minden csoport** panelen, jelölje be hello **Hozzáadás** parancsot.

   ![Hello hozzáadása paranccsal](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. A hello **csoport** panelen név és leírás hello csoport hozzáadása.
6. tooselect tagok tooadd toohello csoportban válassza **hozzárendelt** a hello **tagságtípusának** mezőbe, majd válassza ki **tagok**. Hogyan a toomanage hello csoport tagságának dinamikusan kapcsolatos további információkért lásd: [attribútumok toocreate használata speciális szabályok csoporttagság](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Tagok tooadd kiválasztása](./media/active-directory-groups-create-azure-portal/select-members.png)
7. A hello **tagok** panelen, válasszon ki egy vagy több felhasználók vagy eszközök tooadd toohello csoport és select hello **válasszon** gomb hello panel tooadd hello alján őket toohello csoport. Hello **felhasználói** szűri hello megjelenítési megfelelő a bejegyzés tooany része egy felhasználó vagy eszköz nevét. Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.
8. Amikor befejezte a tagok toohello csoport hozzáadása, válassza ki a **létrehozása** a hello **csoport** panelen.    

   ![Hozzon létre a csoport megerősítése](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagjai kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport tagságát kezelése](active-directory-groups-membership-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
