---
title: "aaaManage integrációs fiók összetevő metaadat - Azure Logic Apps |} Microsoft Docs"
description: "Adja hozzá vagy integrációs fiókok az Azure Logic Apps összetevő metaadatok lekérése"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8de71bffa9f9975d5409716b2208fa6c3a9545d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Integrációs fiókok logic Apps alkalmazásokat a hitelesítendő metaadatok kezelése

Integrációs fiókok egyéni metaadatait az összetevők ad meg, és a metaadatok lekérése futtatás során a logikai alkalmazásnak. Például megadhatja, hogy az összetevők, például a partnerek, egyezmények, sémák és maps metaadat - összes metaadatok kulcs-érték párok tárolására. Jelenleg az összetevők nem hozható létre a metaadatok felhasználói felületén keresztül, de használhatja a REST API-k toocreate metaadatok. tooadd metaadatok létrehozásakor, vagy jelöljön ki egy partner, a szerződés vagy a séma hello Azure-portálon válassza **módosítsa a JSON**. tooretrieve összetevő metaadatokat a logic apps szolgáltatással hello integrációs Fiókkeresés összetevő.

## <a name="add-metadata-tooartifacts-in-integration-accounts"></a>Adja hozzá a metaadatok tooartifacts integrációs fiókok

1. Hozzon létre egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md).

2. Vegyen fel egy összetevő tooyour integrációs fiókot, például egy [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [megállapodás](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), vagy [séma](logic-apps-enterprise-integration-schemas.md).

3.  Válassza ki a hello összetevő, válassza a **módosítsa a JSON**, és írja be a metaadatok adatait.

    ![Adja meg a metaadatok](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>A logic apps összetevőkhöz metaadatok lekérése

1. Hozzon létre egy [logikai alkalmazás](logic-apps-create-a-logic-app.md).

2. Hozzon létre egy [a hivatkozás leválasztása a logic app tooyour integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app). 

3. Logic App Designer, adja hozzá egy eseményindító például *kérelem* vagy *HTTP* tooyour logikai alkalmazást.

4.  Válasszon **tovább** > **művelet hozzáadása**. Keresse meg *integrációs* , keresse meg és jelölje **integrációs fiók - integráció összetevő Fiókkeresés**.

    ![Válassza ki az integráció összetevő Fiókkeresés](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Jelölje be hello **összetevő típusa**, és adja meg a hello **Adatösszetevőt nevét**.

    ![Összetevő típusa, és adja meg az összetevő neve](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Példa: Beolvasni az erőforráspartner adatokat

Partner metaadatok rendelkezik, ezek `routingUrl` részletek:

![Partner "routingURL" metaadatok keresése](media/logic-apps-enterprise-integration-metadata/image6.png)

1. A Logic Apps alkalmazást, adja hozzá az eseményindító egy **integrációs fiók - integráció összetevő Fiókkeresés** művelet esetén a partner és egy **HTTP**.

    ![Eseményindító, a keresési összetevő és a "HTTP" tooyour logikai alkalmazás hozzáadása](media/logic-apps-enterprise-integration-metadata/image4.png)

2. tooretrieve hello URI, nyissa meg a logikai alkalmazásnak nézet tooCode. A logic app-definíciót a példához hasonlóan kell kinéznie:

    ![Keresési keresési](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Következő lépések
* [További információ a megállapodások](logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  
