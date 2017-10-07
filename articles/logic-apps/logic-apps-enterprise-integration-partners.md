---
title: "az üzleti vállalatközi (B2B) üzenetek - Azure Logic Apps aaaCreate partnerek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd partnerek tooyour integrációs hello vállalati integrációs csomag és a Logic Apps fiók"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8dc70a8f441fcf228ed178029dcdbac940d794b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Hozzáadásakor vagy módosításakor a partnerek a munkafolyamat vállalatok szerződések

A partnerek olyan entitásokat, üzleti vállalatközi (B2B) tranzakciók részt, és az exchange-üzenetek között. Létrehozhat, és ezek a tranzakciók a másik szervezet képviselő partnerekkel, előtt is, amely azonosítja, és ellenőrzi az egyes küldött üzenetek az információkat. Miután vitassa meg ezeket az adatokat, és készen áll a toostart az üzleti kapcsolat, az integráció fiók toorepresent partnerek hozhat létre, mindkét.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Milyen szerepkörök rendelkeznek partnerek integrációs fiókja?

partnerek között váltott köszönőüzenetei toodefine részleteit, ezeket a partnerek között létrejött megállapodások létrehozása. Azonban létrehozhat egy szerződést, mielőtt hozzá kellett adnia legalább két partner tooyour integrációs fiók. A szervezet hello hello megállapodás részét kell képeznie **fogadó partner**. másik partnert hello vagy **Vendég partner** jelöli, mely a szervezet üzenetek szervezet hello. hello Vendég partner lehet egy másik vállalat, vagy akár egy részleget a saját szervezetében.

Miután hozzáadta a partnerek, létrehozhat egy szerződést.

Értesítések fogadásához és küldéséhez beállítások hello szempontból a birtokolt Partner hello irányulnak. Például hello kap a szerződés beállítások határozza meg, hogyan üzemeltetett hello partner kap egy Vendég partner küldött üzenetek. Hasonlóképpen hello küldési beállítások hello megállapodás azt jelzik, hogyan üzemeltetett hello partner küld üzeneteket toohello Vendég partner.

## <a name="how-toocreate-a-partner"></a>Hogyan toocreate partner?

1. Hello Azure-portálon, válassza ki **Tallózás**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Hello szűrő keresési mezőbe, írja be a **integrációs**, majd jelölje be **integrációs fiókok** hello eredménylistában.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Válassza ki a hello integrációs fiókra, amelyhez tooadd partnereivel.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Jelölje be hello **partnerek** csempére.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Hello partnerek paneljén válassza **Hozzáadás**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Adja meg a partner nevét, majd válasszon egy **minősítő**. Végül adja meg a **érték** toohelp dokumentumok, amelyek az alkalmazások azonosítása.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. a partner létrehozási folyamata, jelölje be hello toosee hello folyamatban *harang* értesítési ikon.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. tooconfirm, amely az új partnerek sikeresen létre lettek hozzáadva, jelölje be hello **partnerek** csempére.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    Hello partnerek csempe kiválasztása után is megjelenik az újonnan hozzáadott partnerek hello partnerek panelen.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-tooedit-existing-partners-in-your-integration-account"></a>Hogyan tooedit meglévő partnereknek az integráció-fiókban

1. Jelölje be hello **partnerek** csempére.
2. Ha megnyílt az hello partnerek panelen, válassza ki a tooedit kívánt hello partnert.
3. A hello **frissítés Partner** csempére, hajtsa végre a módosításokat.
4. Miután elkészült, válassza ki azt **mentése**, toocancel a módosításokat, válassza ki vagy **elvetése**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-toodelete-a-partner"></a>Hogyan toodelete partner

1. Jelölje be hello **partnerek** csempére.
2. Miután hello Partner panel nyílik meg, válassza ki a megjeleníteni kívánt toodelete hello partnert.
3. Válasszon **törlése**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Következő lépések
* [További információ a megállapodások](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  

