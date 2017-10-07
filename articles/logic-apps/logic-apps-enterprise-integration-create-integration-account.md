---
title: "aaaCreate, csatolja, törléséhez vagy egy integrációs fiók áthelyezése az Azure logic apps |} Microsoft Docs"
description: "Hogyan toocreate integrációs fiókot, és hivatkozás tooyour logic Apps alkalmazások"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: fda6c91723b3e3624ee176df112ba8b6c9800273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-integration-account"></a>Mi az az integráció fiókkal?

Integráció fiók lehetővé teszi a vállalati integrációs alkalmazások toomanage összetevők, beleértve a sémák, térképeket, tanúsítványok, partnerek és szerződéseket. Bármely integrációs alkalmazást hoz létre egy integrációs fiók tooaccess használ, a sémák, térképeket, tanúsítványok és stb.

## <a name="create-an-integration-account"></a>Integráció-fiók létrehozása

1.  Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com "Azure-portálon"). Hello bal oldali menüben válassza ki a **további szolgáltatások**.

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz. Hello eredmények listában válassza ki a **integrációs fiókok**.

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Hello hello oldal tetején, válassza ki a **Hozzáadás**.

    ![Válasszon hozzáadása](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Integráció fiókja és -válassza hello Azure-előfizetést, amelyet az toouse nevet. Létrehozhat egy új **erőforráscsoport** vagy válasszon ki egy meglévő erőforráscsoportot. Válassza ki a **hely** integrációs fiókja üzemeltetésére és egy **árképzési szintjében**. 

    Ha elkészült, válassza ki a **létrehozása**.

    ![Részletek a integrációs fiók megadása](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure látja el a integrációs fiók hello kiválasztott helyen, amely 1 percen belül kell végrehajtani.

5. Frissítse a hello lapot. Megjelenik az új integrációs fiókját.

    ![Új integrációs fiókja jelenik meg.](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

A következő fiókját hello integrációs adott létrehozott tooyour logikai alkalmazást. 

## <a name="link-an-integration-account-tooa-logic-app"></a>Hivatkozásra egy integrációs fiók tooa logikai alkalmazás

toogive a logic apps toomaps, sémákat, szerződések és egyéb összetevők integrációs fiókjában, hivatkozás hello integrációs fiók tooyour logikai alkalmazás eléréséhez.

### <a name="prerequisites"></a>Előfeltételek

* Integráció fiók
* A logikai alkalmazás

> [!NOTE] 
> Gondoskodjon arról, hogy az integrációs fiók és a logic app hello *azonos Azure-beli hely* megkezdése előtt.


1. Hello Azure-portálon válassza ki a Logic Apps alkalmazást, és ellenőrizze a logikai alkalmazás helyét.

    ![Válassza ki a Logic Apps alkalmazást, a hely ellenőrzése](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. A **beállítások**, jelölje be **integrációs fiók**.

    ![Válassza ki a "Integrációs fiók"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. A hello **válassza ki a integrációs fiókot** listájában, jelölje be hello integrációs fióknevet, amelyet a toolink tooyour logikai alkalmazást. linking, toofinish válassza **mentése**.

    ![Válassza ki a integrációs fiókját](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Bemutatja az integráció fiók csatolt tooyour logic app, valamint arról, hogy az integrációs-fiókban lévő összes összetevők most elérhető tooyour logikai alkalmazás értesítést kap.

    ![A Logic Apps alkalmazást kapcsolódó tooyour integrációs fiók](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Most, hogy az integráció fiók csatolt tooyour logic app, hello B2B összekötők a logic apps segítségével is. Néhány gyakori B2B összekötők XML-érvényesítés, és egybesimított fájl kódolási/dekódolási tartalmazza.  

## <a name="delete-your-integration-account"></a>Az integráció fiók törlése

1. Válassza ki **további szolgáltatások**.

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz. Hello eredmények listában válassza ki a **integrációs fiókok**.

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Válassza ki, hogy szeretné-e toodelete hello integrációs fiókot.

    ![Válassza ki az integráció fiók toodelete](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. A hello menüben válasszon **törlése**.

    ![Válassza a "Delete"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. A choice toodelete hello integrációs visszaigazoláshoz.

## <a name="move-your-integration-account"></a>Az integráció fiók áthelyezése

toomove egy integrációs fiók tooanother Azure előfizetés vagy az erőforrás-csoportok, kövesse az alábbi lépéseket.

> [!IMPORTANT]
> Integráció fiók áthelyezése után frissítenie kell az összes parancsfájlok toouse hello új erőforrás-azonosítók.

1. Válassza ki **további szolgáltatások**.

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz. Hello eredmények listában válassza ki a **integrációs fiókok**.

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Válassza ki, hogy szeretné-e toomove hello integrációs fiókot. A **beállítások**, válassza a **tulajdonságok**.

    ![Válassza ki az integráció fiók toomove. A beállítások területen válassza a Tulajdonságok](./media/logic-apps-enterprise-integration-accounts/move.png)

5. Hello erőforráscsoport vagy az Azure-integráció fiókjához tartozó előfizetés módosítása.

    ![Válassza ki a módosítás erőforráscsoportba vagy módosítás előfizetés](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Következő lépések
* [További információ a megállapodások](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  

