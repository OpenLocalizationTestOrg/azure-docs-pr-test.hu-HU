---
title: "aaaDisaster helyreállítási B2B integrációs fiók - Azure Logic Apps |} Microsoft Docs"
description: "Logic Apps B2B katasztrófa utáni helyreállítás"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e86564a3c5a2607d22514936c606e2843cba0416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B kereszt-régió katasztrófa utáni helyreállítás

B2B munkaterhelések vonatkozhat például rendeléseket és a számlák pénz tranzakciók. A vész-események esetén fontos az olyan üzleti tooquickly helyreállítás toomeet hello a partnerekkel együttműködve egyeztetett vállalati szintű SLA-k. Ez a cikk bemutatja, hogyan toobuild üzletmenet-folytonosságának megtervezése B2B munkaterhelések. 

* Vész-helyreállítási készültségi 
* Toosecondary régió feladatátvételt egy vész-esemény 
* Tartalék megoldásként tooprimary régió katasztrófa esemény után

## <a name="disaster-recovery-readiness"></a>Vész-helyreállítási készültségi  

1. Egy másodlagos régióban azonosításához, és hozzon létre egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) hello másodlagos régióban.

2. Partnerek, a sémák és a szükséges hello üzenet adatfolyamok, amikor a futtatási állapot hello kell replikálni toobe toosecondary régió integrációs fiók megállapodások hozzáadása.

   > [!TIP]
   > Ellenőrizze, hogy nincs konzisztencia hello integrációs fiók összetevő elnevezési régiók közötti. 

3. futtatási állapot hello elsődleges régióban, toopull hello hello másodlagos régióban logikai alkalmazás létrehozása. 

   A logikai alkalmazás rendelkeznie kell egy *eseményindító* és egy *művelet*. 
   hello eseményindító csatlakoznia tooprimary régió integrációs fiókot, és hello művelet toosecondary régió integrációs fiókot kell csatlakozniuk. 
   Hello időtartam alapján, hello eseményindító hello futtatása elsődleges régió állapot tábla kérdezi le, és lekéri a hello új rekordok, ha van ilyen. hello művelet frissíti őket toosecondary régió integrációs fiók. 
   Ezzel a megoldással tooget növekményes futási állapotának elsődleges régió toosecondary régióban.

4. A Logic Apps-integráció fiók az üzletmenet folytonossága B2B protokollokat - X12, és a AS2, EDIFACT toosupport tervezték. toofind részletes lépései, válassza ki a megfelelő hivatkozásokra hello.

5. a javaslat hello túlságosan toodeploy összes elsődleges régió erőforrást egy másodlagos régióban. 

   Elsődleges régió erőforrások közé tartoznak, az Azure SQL Database vagy Azure Cosmos DB, Azure Service Bus és az Azure Event Hubs üzenetküldési, az Azure API Management és az Azure App Service hello Azure Logic Apps szolgáltatás használt.   

6. A kapcsolatot az elsődleges régióban tooa másodlagos régióba. futtatási állapot egy elsődleges régióban toopull hello logikai alkalmazás létrehozása egy másodlagos régióban. 

   hello logikai alkalmazás rendelkeznie kell egy eseményindító és egy műveletet. 
   hello eseményindító tooa elsődleges régió integrációs fiók csatlakoznia. 
   hello művelet tooa másodlagos régióba integrációs fiók csatlakoznia. 
   Hello időtartam alapján, hello eseményindító hello futtatása elsődleges régió állapot tábla kérdezi le, és lekéri a hello új rekordok, ha van ilyen. 
   hello művelet frissíti őket tooa másodlagos régióba integrációs fiók. 
   Ez az eljárás tooget növekményes futási állapotának segít hello elsődleges régió toohello másodlagos régióban.

A Logic Apps-integráció fiókban az üzletmenet folytonossága támogatja az AS2, EDIFACT és a hello B2B protokollok X12 alapján. A X12 és az AS2 részletes lépéseiért lásd: [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) és [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) ebben a cikkben.

## <a name="fail-over-tooa-secondary-region-during-a-disaster-event"></a>Tooa másodlagos régióba feladatátvételt egy vész-esemény

A vészhelyreállítás események, amikor hello elsődleges régió nem érhető el az üzletmenet folytonosságának, közvetlen forgalom toohello másodlagos régióba. Egy másodlagos régióban segítségével gyorsan toomeet hello RPO/Helyreállításiidő megegyezésre által a partnerek működik egy üzleti toorecover. Is minimalizálja erőfeszítéseket toofail keresztül egy régió tartozik tooanother régióban. 

Nincs olyan várható késleltetés vezérlő számok másolása egy elsődleges régió tooa másodlagos régióba. ismétlődő létrehozott vezérlő küldése tooavoid toopartners számok katasztrófa esemény során, a hello ajánlás tooincrement hello vezérlő számok hello másodlagos régióba megállapodásokban használatával [PowerShell-parancsmagok](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-tooa-primary-region-post-disaster-event"></a>Tartalék megoldásként tooa elsődleges régió katasztrófa utáni esemény

toofall hátsó tooa elsődleges régió a rendelkezésre álló, kövesse az alábbi lépéseket:

1. Hello másodlagos régióban partnerektől származó üzenetek fogadását.  

2. Használatával növelheti a generált hello vezérlő számokat az összes hello elsődleges régió szerződéshez [PowerShell-parancsmagok](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Hello másodlagos régióba toohello elsődleges régió közvetlen forgalmát.

4. Ellenőrizze, hogy a futtatási állapot hello elsődleges régióban húzza hello másodlagos régióban létrehozott hello logikai alkalmazás engedélyezve van.

## <a name="x12"></a>X12 

X 12 dokumentumok EDI üzletmenet folytonossága vezérlő számok alapján:

> [!TIP]
> Is használhatja a hello [X12 quick start sablon](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic Apps alkalmazásokat. Létrehozás elsődleges és másodlagos integrációs fiókok Előfeltételek toouse hello sablon. hello sablon segítségével toocreate két a logic apps, kapott vezérlő számok egyet, majd egy másikat a létrehozott vezérlő számokat. Megfelelő eseményindítók és műveletek hello a logic apps, csatlakozás hello eseményindító toohello elsődleges integrációs fiók és a másodlagos integrációs toohello hello műveletfiókjához jönnek létre.

**Előfeltételek**

bejövő üzenetek tooenable vész-helyreállítási beállításban válassza ismétlődő hello beállításokat hello X12 megállapodás fogadására.

![Válassza ki a duplikált beállításokat](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Hozzon létre egy [logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) egy másodlagos régióban.    

2. A Keresés **X12**, és válassza ki **X12-ellenőrzési számot módosításakor**.   

   ![X12 keresése](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   hello eseményindító tooestablish egy kapcsolat tooan integrációs fiók megadását kéri. 
   hello eseményindító kell csatlakoztatott tooa elsődleges régió integrációs fiók.

3. Adja meg a kapcsolat nevét, válassza ki a *elsődleges régió integrációs fiók* hello a listában, és válassza a **létrehozása**.   

   ![Elsődleges régió integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. Hello **DateTime toostart vezérlő számú szinkronizálási** beállítás nem kötelező. Hello **gyakoriság** túl beállítható**nap**, **óra**, **perc**, vagy **második** időközzel.   

   ![Dátum és idő és gyakorisága](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Válassza ki **új lépés** > **művelet hozzáadása**.

   ![Új lépés, Action hozzáadása](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. A Keresés **X12**, és válassza ki **X12-hozzáadásakor vagy módosításakor a vezérlő számok**.   

   ![Hozzáadásakor vagy módosításakor a vezérlő számok](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. tooconnect egy műveleti tooa másodlagos régióba integrációs fiókot válasszon **kapcsolat módosítása** > **új kapcsolat hozzáadása** hello elérhető integrációs fiókok listáját. Adja meg a kapcsolat nevét, válassza ki a *másodlagos régióba integrációs fiók* hello a listában, és válassza a **létrehozása**. 

   ![Másodlagos régióba integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Váltás tooraw bemenetek jobb felső sarkában található hello ikonra kattintva.

   ![Kapcsoló tooraw bemenetek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Válassza ki a szervezet hello dinamikus tartalom objektumválasztó a, és mentse a hello logikai alkalmazás.

   ![Dinamikus tartalom mezők](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Hello időtartam alapján, hello eseményindító kérdezze le az hello kapott elsődleges régió vezérlő tábla száma és hello új rekordok lekérése. 
   hello művelet frissíti a hello másodlagos régióba integrációs fiók hello rekordokat. 
   Ha a nincsenek frissítések, hello eseményindító állapota megjelenik **kimaradnak**.   

   ![Vezérlő tábla száma](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Hello időtartam alapján, hello növekményes futási állapotának replikálja az elsődleges régióban tooa másodlagos régióba. A vészhelyreállítás események, ha hello elsődleges régióban nem áll rendelkezésre, közvetlen forgalom toohello másodlagos régióba az üzletmenet folytonossága érdekében. 

## <a name="edifact"></a>EDIFACT 

Az üzletmenet folytonossága EDI EDIFACT-dokumentumok vezérlő számok alapul.

**Előfeltételek**

bejövő üzenetek tooenable vész-helyreállítási beállításban válassza ismétlődő hello beállításokat a EDIFACT megállapodás fogadására.

![Válassza ki a duplikált beállításokat](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Hozzon létre egy [logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) egy másodlagos régióban.    

2. A Keresés **EDIFACT**, és válassza ki **EDIFACT - ellenőrzési számot módosításakor**.

   ![EDIFACT keresése](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   hello eseményindító tooestablish egy kapcsolat tooan integrációs fiók megadását kéri. 
   hello eseményindító kell csatlakoztatott tooa elsődleges régió integrációs fiók. 

3. Adja meg a kapcsolat nevét, válassza ki a *elsődleges régió integrációs fiók* hello a listában, és válassza a **létrehozása**.    

   ![Elsődleges régió integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. Hello **DateTime toostart vezérlő számú szinkronizálási** beállítás nem kötelező. Hello **gyakoriság** túl beállítható**nap**, **óra**, **perc**, vagy **második** időközzel.    

   ![Dátum és idő és gyakorisága](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Válassza ki **új lépés** > **művelet hozzáadása**.    

   ![Új lépés, Action hozzáadása](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. A Keresés **EDIFACT**, és válassza ki **EDIFACT - hozzáadásakor vagy módosításakor a vezérlő számok**.   

   ![Hozzáadásakor vagy módosításakor a vezérlő számok](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. tooconnect egy műveleti tooa másodlagos régióba integrációs fiókot válasszon **kapcsolat módosítása** > **új kapcsolat hozzáadása** hello elérhető integrációs fiókok listáját. Adja meg a kapcsolat nevét, válassza ki a *másodlagos régióba integrációs fiók* hello a listában, és válassza a **létrehozása**.

   ![Másodlagos régióba integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Váltás tooraw bemenetek jobb felső sarkában található hello ikonra kattintva.

   ![Kapcsoló tooraw bemenetek](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Válassza ki a szervezet hello dinamikus tartalom objektumválasztó a, és mentse a hello logikai alkalmazás.   

   ![Dinamikus tartalom mezők](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Hello időtartam alapján, hello eseményindító kérdezze le az hello kapott elsődleges régió vezérlő tábla száma és hello új rekordok lekérése.
   hello művelet frissíti a hello rekordok toohello másodlagos régióba integrációs fiók. 
   Ha a nincsenek frissítések, hello eseményindító állapota megjelenik **kimaradnak**.

   ![Vezérlő tábla száma](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Hello időtartam alapján, hello növekményes futási állapotának replikálja az elsődleges régióban tooa másodlagos régióba. A vészhelyreállítás események, ha hello elsődleges régióban nem áll rendelkezésre, közvetlen forgalom toohello másodlagos régióba az üzletmenet folytonossága érdekében. 

## <a name="as2"></a>AS2 

Az üzletmenet folytonossága hello AS2 protokollt használó dokumentumok hello Üzenetazonosítója és hello MIC érték alapul.

> [!TIP]
> Is használhatja a hello [AS2 gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic Apps alkalmazásokat. Létrehozás elsődleges és másodlagos integrációs fiókok Előfeltételek toouse hello sablon. hello sablon segítségével, amelyen egy eseményindító és művelet logikai alkalmazás létrehozása. hello logikai alkalmazás kapcsolatot hoz létre egy eseményindító tooa elsődleges integrációs fiókot és egy művelet tooa másodlagos integrációs fiókhoz.

1. Hozzon létre egy [logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) hello másodlagos régióban.  

2. A Keresés **AS2**, és válassza ki **AS2 - Ha a MIC érték jön létre**.   

   ![AS2 keresése](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Egy eseményindító tooestablish egy kapcsolat tooan integrációs fiók kéri. 
   hello eseményindító kell csatlakoztatott tooa elsődleges régió integrációs fiók. 
   
3. Adja meg a kapcsolat nevét, válassza ki a *elsődleges régió integrációs fiók* hello a listában, és válassza a **létrehozása**.

   ![Elsődleges régió integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. Hello **DateTime toostart MIC érték szinkronizálási** beállítás nem kötelező. Hello **gyakoriság** túl beállítható**nap**, **óra**, **perc**, vagy **második** időközzel.   

   ![Dátum és idő és gyakorisága](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Válassza ki **új lépés** > **művelet hozzáadása**.  

   ![Új lépés, Action hozzáadása](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. A Keresés **AS2**, és válassza ki **AS2 - hozzáadásakor vagy módosításakor a MIC tartalma**.  

   ![MIC hozzáadása vagy frissítése](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. tooconnect egy műveleti tooa másodlagos integrációs fiókot válasszon **kapcsolat módosítása** > **új kapcsolat hozzáadása** hello elérhető integrációs fiókok listáját. Adja meg a kapcsolat nevét, válassza ki a *másodlagos régióba integrációs fiók* hello a listában, és válassza a **létrehozása**.

   ![Másodlagos régióba integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Váltás tooraw bemenetek jobb felső sarkában található hello ikonra kattintva.

   ![Kapcsoló tooraw bemenetek](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Válassza ki a szervezet hello dinamikus tartalom objektumválasztó a, és mentse a hello logikai alkalmazás.   

   ![Dinamikus tartalom](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Hello alatt az időtartam alatt, hello eseményindító kérdezze le az elsődleges régió tábla hello és hello új rekordok kéri le. hello művelet frissíti őket toohello másodlagos régióba integrációs fiók. 
   Ha a nincsenek frissítések, hello eseményindító állapota megjelenik **kimaradnak**.  

   ![Elsődleges régió tábla](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Hello növekményes futási állapotának hello elsődleges régió toohello másodlagos régióban hello időtartam alapján, replikálja. A vészhelyreállítás események, ha hello elsődleges régióban nem áll rendelkezésre, közvetlen forgalom toohello másodlagos régióba az üzletmenet folytonossága érdekében. 

## <a name="next-steps"></a>Következő lépések

[B2B üzenetek megfigyelése](logic-apps-monitor-b2b-message.md)

